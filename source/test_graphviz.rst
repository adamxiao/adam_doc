Welcome to adam_doc's documentation!
====================================

.. graphviz::
   :alt: HTTP Transaction State Diagram

   digraph http_txn_state_diagram{
     accept -> TS_HTTP_TXN_START_HOOK;
     TS_HTTP_TXN_START_HOOK -> "read req hdrs";
     "read req hdrs" -> TS_HTTP_READ_REQUEST_HDR_HOOK;
     TS_HTTP_READ_REQUEST_HDR_HOOK -> TS_HTTP_PRE_REMAP_HOOK;
     TS_HTTP_PRE_REMAP_HOOK -> "remap request";
     "remap request" -> TS_HTTP_POST_REMAP_HOOK;
     TS_HTTP_POST_REMAP_HOOK -> "cache lookup";
     "cache lookup" -> DNS [label = "miss"];
     DNS -> TS_HTTP_OS_DNS_HOOK;
     TS_HTTP_OS_DNS_HOOK -> TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK;
     "cache lookup" -> TS_HTTP_SELECT_ALT_HOOK [label = "hit"];
     TS_HTTP_SELECT_ALT_HOOK -> "cache match";
     "cache match" -> TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK [label="no match"];
     "cache match" -> TS_HTTP_READ_CACHE_HDR_HOOK [label = "cache fresh"];
     TS_HTTP_READ_CACHE_HDR_HOOK -> "cache fresh";
     "cache fresh" -> TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK;
     TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK -> "lock URL in cache" [label = "miss"];
     TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK -> "lock URL in cache" [label = "no match  "];
     TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK -> "lock URL in cache" [label = "stale"];
     TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK -> "send cached hdrs" [label = "fresh"];
     "send cached hdrs" -> "set up transform";
     "lock URL in cache" -> "pick address";
     "pick address" -> "try connect" [label = "       "];
     "try connect" -> "pick address" [label = "fail"];
     "try connect" -> TS_HTTP_SEND_REQUEST_HDR_HOOK [label = "success"];
     TS_HTTP_SEND_REQUEST_HDR_HOOK -> "send req hdrs";
     "send req hdrs" -> "set up POST/PUT read" [label = "POST/PUT"];
     "send req hdrs" -> "read reply hdrs" [label = "GET"];
     "set up POST/PUT read" -> "set up req transform";
     "set up req transform" -> "tunnel req body";
     "tunnel req body" -> "read reply hdrs";
     "read reply hdrs" -> TS_HTTP_READ_RESPONSE_HDR_HOOK;
     TS_HTTP_READ_RESPONSE_HDR_HOOK -> "check valid";
     "check valid" -> "setup server read" [label = "yes"];
     "check valid" -> "pick address" [label = "no"];
     "setup server read" -> "set up cache write" [label = "cacheable"];
     "setup server read" -> "set up transform" [label = "uncacheable"];
     "set up cache write" -> "set up transform";
     "set up transform" -> TS_HTTP_SEND_RESPONSE_HDR_HOOK;
     TS_HTTP_SEND_RESPONSE_HDR_HOOK -> "send reply hdrs";
     "send reply hdrs" -> "tunnel response";
     "tunnel response" -> TS_HTTP_TXN_CLOSE_HOOK;
     TS_HTTP_TXN_CLOSE_HOOK -> accept;
    
     TS_HTTP_TXN_START_HOOK [shape=box];
     TS_HTTP_READ_REQUEST_HDR_HOOK [shape = box];
     TS_HTTP_PRE_REMAP_HOOK [shape = box];
     TS_HTTP_POST_REMAP_HOOK [shape = box];
     TS_HTTP_OS_DNS_HOOK [shape = box];
     TS_HTTP_CACHE_LOOKUP_COMPLETE_HOOK[shape = box];
     TS_HTTP_SELECT_ALT_HOOK [shape = box];
     TS_HTTP_READ_CACHE_HDR_HOOK [shape = box];
     TS_HTTP_SEND_REQUEST_HDR_HOOK [shape = box];
     "set up req transform" [tooltip = "req transform takes place here"];
     TS_HTTP_READ_RESPONSE_HDR_HOOK [shape = box];
     "set up transform" [tooltip = "response transform takes place here"];
     TS_HTTP_SEND_RESPONSE_HDR_HOOK [shape = box];
     TS_HTTP_TXN_CLOSE_HOOK [shape = box];
   }

