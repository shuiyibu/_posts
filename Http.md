---
layout: c++
title: Http
date: 2017-09-29 09:52:58
tags:

---





```c++
#ifndef httpclient_h
#define httpclient_h
class HttpClient
{
public:
    HttpClient();
    ~HttpClient();
    void DebugOut(const char *fmt, ...);
    
    int HttpGet(const char* strUrl, char* strResponse);
    int HttpPost(const char* strUrl, const char* strData, char* strResponse);
    
private:
    int   HttpRequestExec(const char* strMethod, const char* strUrl, const char* strData, char* strResponse);
    char* HttpHeadCreate(const char* strMethod, const char* strUrl, const char* strData);
    char* HttpDataTransmit(char *strHttpHead, const int iSockFd);
    
    int   GetPortFromUrl(const char* strUrl);
    char* GetIPFromUrl(const char* strUrl);
    char* GetParamFromUrl(const char* strUrl);
    char* GetHostAddrFromUrl(const char* strUrl);
    
    int   SocketFdCheck(const int iSockFd);
    
    static int m_iSocketFd;
};
#endif /* httpclient_h */
```

