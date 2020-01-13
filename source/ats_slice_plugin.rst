ats slice plugin
====================================

data flow
------------------------------------

.. graphviz::
   :alt: qemu deliver data flow

   digraph G {
 
     node [shape=box]

     "ats开始处理请求" -> "取源的HTTP请求增加Range头部" [label="分片请求"];
     "取源的HTTP请求增加Range头部" -> "将源的响应发给客户端";
     "将源的响应发给客户端" -> "状态码不为206" ;
     "状态码不为206" -> "请求结束" [label="是"];
     "状态码不为206" -> "到达文件末尾" [label="否"];
     "到达文件末尾" -> "生成子请求，向源请求下一个分片" [label="否"];
     "到达文件末尾" -> "请求结束" [label="是"];
     "生成子请求，向源请求下一个分片" -> "取源的HTTP请求增加Range头部" [label="继续分片请求"];

     "状态码不为206" [shape=diamond];
     "到达文件末尾" [shape=diamond];

   }
