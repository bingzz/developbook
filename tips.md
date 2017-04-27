
#### swift中使用markdown

    editor-show rendered markup
    代码中/\*:  ...  \*/
    
#### webview中链接编码问题

 - javascript encodeURI() <=>  stringByAddingPercent 
   用于整个uri的编码
   
         http://www.oschina.net/search?scope=bbs&q=C语言
         ->http://www.oschina.net/search?scope=bbs&q=C%E8%AF%AD%E8%A8%80 
         
 - js: encodeURIComponents() 
   用于参数拼接url编码  
   
        http://www.oschina.net/search?scope=bbs&q=C语言
        ->http%3A%2F%2Fwww.oschina.net%2Fsearch%3Fscope%3Dbbs%26q%3DC%E8%AF%AD%E8%A8%80
        
以上两种编码 都可以通过`stringByRemovingPercentEncoding`解码








