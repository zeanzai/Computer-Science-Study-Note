---
layout: post
title:  "跨域问题"
date:   2018-06-05 00:42:27 +0800
author: zeanzai
tags:
    - QA
    - Exception
---

# 关于跨域问题

使用框架是SpringBoot+Shiro框架，需要更改两个地方： 

```Java
public class XssFilter implements Filter {

    @Override
    public void init(FilterConfig config) throws ServletException {
    }

    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        XssHttpServletRequestWrapper xssRequest = new XssHttpServletRequestWrapper(
                (HttpServletRequest) req);
        HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Expose-Headers", "token");
        response.setHeader("Access-Control-Allow-Origin", "*");     // 允许所有域都可以访问
        response.setHeader("Access-Control-Allow-Methods", "*"); // or *
        response.setHeader("Access-Control-Allow-Headers", "Origin, No-Cache, X-Requested-With, If-Modified-Since, Pragma, Last-Modified, Cache-Control, Expires, Content-Type, X-E4M-With, userId, token"); // or *
        response.setHeader("Access-Control-Max-Age", "86400");  // 24h
        chain.doFilter(xssRequest, response);
    }

    @Override
    public void destroy() {
    }

}
```

```Java
public class OAuth2Filter extends AuthenticatingFilter {

    /**
     * 如果token为空，就生成一个OAuth2token
     * @author      w.x.y
     * @date        2017/12/4 11:15
     * @version     1.0
     * @modified
     */
    @Override
    protected AuthenticationToken createToken(ServletRequest request, ServletResponse response) throws Exception {
        //获取请求token
        String token = this.getRequestToken((HttpServletRequest) request);

        if(StringUtils.isBlank(token)){
            return null;
        }

        return new OAuth2Token(token);
    }

    /**
     * 允许登录
     * @author      w.x.y
     * @date        2017/12/4 11:16
     * @version     1.0
     * @modified
     */
    @Override
    protected boolean isAccessAllowed(ServletRequest request, ServletResponse res, Object mappedValue) {
        HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Expose-Headers", "token");                                   // 请求头中的token
        response.setHeader("Access-Control-Allow-Origin", "*");                                         // 允许所有域都可以访问
        response.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS, HEAD");    // or *
        response.setHeader("Access-Control-Allow-Headers", "Origin, No-Cache, X-Requested-With, If-Modified-Since, Pragma, " +
				"Last-Modified, Cache-Control, Expires, Content-Type, X-E4M-With, userId, token");				// or *
        response.setHeader("Access-Control-Max-Age", "86400");                                          // 24小时
        response.setContentType("application/json");
        return false;
    }

    /**
     * 不允许登录。
     *      执行登录界面
     * @author      w.x.y
     * @date        2017/12/4 11:16
     * @version     1.0
     * @modified
     */
    @Override
    protected boolean onAccessDenied(ServletRequest request, ServletResponse response) throws Exception {
        //获取请求token，如果token不存在，直接返回401
        String token = getRequestToken((HttpServletRequest) request);
        if(StringUtils.isBlank(token)){
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            String json = new Gson().toJson(ResultMsg.error(HttpStatus.SC_UNAUTHORIZED, "invalid token"));
            httpResponse.getWriter().print(json);

            return false;
        }

        return super.executeLogin(request, response);
    }

    /**
     * 登录失败
     * @author      w.x.y
     * @date        2017/12/4 11:18
     * @version     1.0
     * @modified
     */
    @Override
    protected boolean onLoginFailure(AuthenticationToken token, AuthenticationException e, ServletRequest request, ServletResponse response) {
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setContentType("application/json;charset=utf-8");
        try {
            //处理登录失败的异常
            Throwable throwable = e.getCause() == null ? e : e.getCause();
            ResultMsg r = ResultMsg.error(HttpStatus.SC_UNAUTHORIZED, throwable.getMessage());

            String json = new Gson().toJson(r);
            httpResponse.getWriter().print(json);
        } catch (IOException e1) {

        }

        return false;
    }

    /**
     * 获取请求的token
     */
    private String getRequestToken(HttpServletRequest httpRequest){
        //从header中获取token
        String token = httpRequest.getHeader("token");

        //如果header中不存在token，则从参数中获取token
        if(StringUtils.isBlank(token)){
            token = httpRequest.getParameter("token");
        }

        return token;
    }


}

```

前端使用的是vue+axois，好像是这个。 

