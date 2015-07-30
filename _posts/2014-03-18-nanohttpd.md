---
layout: post
title:  "Android使用nanohttpd实现手机上的web服务器"
date:   2014-03-18 14:06:05
categories: android
---

* content
{:toc}

#### NanoHttpd介绍

官方网站 [http://nanohttpd.org/](http://nanohttpd.org/)

NanoHttpd is a light-weight HTTP server designed for embedding in other applications.

NanoHttpd has been released under a Modified BSD licence.

#### NanoHttpd使用

    import fi.iki.elonen.NanoHTTPD;
    
    ......
    
    class WebServer extends NanoHTTPD {
        public WebServer()
        {
            super(0);
        }
        
        ......
        
        public Response serve(IHTTPSession session) {
            Method method = session.getMethod();
            if (NanoHTTPD.Method.GET.equals(method))
            {
                Map<String, String> params = session.getParms();

                if (params.containsKey("act") && params.containsKey("name"))
                {
                    String act = params.get("act");
                    String name = params.get("name");

                    if (act.equals("del"))
                    {
                        File file = new File(getDir(getString(R.string.template_dir), MODE_PRIVATE).getPath() + "/" + name);
                        file.delete();
                    }
                }
                return newFixedLengthResponse(Response.Status.OK, MIME_HTML, buildHtml());
            }
            else if (NanoHTTPD.Method.POST.equals(method))
            {
                Map<String, String> posts = new HashMap<String, String>();
                try
                {
                    session.parseBody(posts);
                    Map<String, String> params = session.getParms();
                    Set<String> keys = posts.keySet();
                    String name = "";
                    String location = "";
                    for (String key : keys)
                    {
                        if (key.equals("uploadfile"))
                        {
                            location = posts.get(key);
                        }
                    }
                    if (params.containsKey("name"))
                    {
                        name = params.get("name");
                    }

                    if (name.length() > 0 || location.length() > 0)
                    {
                        File tempFile = new File(location);
                        File destFile = new File(getDir(getString(R.string.template_dir), MODE_PRIVATE).getPath() + "/" + name);
                        copyFile(tempFile.getPath(), destFile.getPath());
                    }

                    return newFixedLengthResponse(Response.Status.OK, MIME_HTML, buildHtml());
                }
                catch (NanoHTTPD.ResponseException | IOException ex)
                {
                    ex.printStackTrace();
                    StringBuffer buffer = new StringBuffer();
                    buffer.append("upload error!");
                    return newFixedLengthResponse(Response.Status.INTERNAL_ERROR, MIME_HTML, buffer.toString());
                }
            }

            return super.serve(session);
        }
        ......
    }