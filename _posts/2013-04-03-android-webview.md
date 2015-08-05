---
layout: post
title:  "Android WebView及web中调用Java"
date:   2013-06-09 14:06:05
categories: Android
---

* content
{:toc}

Java类

    class DevCtrl {
        @android.webkit.JavascriptInterface
        public String sendDevMsg(String msg) {
            try
            {
                InetSocketAddress sockaddr = new InetSocketAddress(mAddr, mPort);
                Socket socket = new Socket();
                socket.connect(sockaddr, 1000);
                socket.setSoTimeout(1000);
                OutputStream os = socket.getOutputStream();
                DataOutputStream dos = new DataOutputStream(os);
                dos.writeBytes(msg);
                InputStream is = socket.getInputStream();
                DataInputStream dis = new DataInputStream(is);
                byte[] resp = new byte[512];
                dis.read(resp);
                String ret = new String(resp);
                dos.close();
                os.close();
                dis.close();
                is.close();
                socket.close();
                return ret;
            }
            catch (UnknownHostException e)
            {
                return getString(R.string.unknown_host);
            }
            catch (IOException e)
            {
                return getString(R.string.network_error);
            }
        }
    }
    
在Activity中设置WebView

        WebView wv = (WebView)findViewById(R.id.webview);
        wv.getSettings().setJavaScriptEnabled(true);
        wv.addJavascriptInterface(new DevCtrl(), "devctrl");
        wv.loadUrl("file://" + getDir(getString(R.string.template_dir), MODE_PRIVATE).getPath() + "/" + mTemplate);

HTML测试页面

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="zh-CN" dir="ltr">
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <script type="text/javascript">
    function sendDevMsg() {
    result = devctrl.sendDevMsg(document.getElementById('result').value);
    document.getElementById('result').value = result;
    }
    </script>
    </head>
    <body>
    <textarea id="result">
    </textarea>
    <input type="button" value="Send Dev Msg"
           onClick="sendDevMsg()" />
    </body>
    </html>
