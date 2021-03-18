---
layout: post
title: "[java]FileCopyUtils, 첨부파일 다운"
date: 2021-03-17 09:37:23 +0900
category: java
---

# [java]FileCopyUtils, 첨부파일 다운

파일 및 스트림 복사를 위한 간단한 유틸리티 메소드의 집합체이다. 

모든 복사 방법은 4096byte의 블록크기를 사용하고 완료되면 모든 영향을 받는 스트림을 닫는다.

```java
import org.springframework.util.FileCopyUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.net.URLEncoder;

public class DownloadView {

    public void filDown(HttpServletRequest request,
                        HttpServletResponse response, String filePath, String realFilNm,
                        String viewFileNm) throws IOException {

        File file = new File( filePath + realFilNm);
        // System.out.println("root = " + filePath + realFilNm);
        if (file.exists() && file.isFile()) {
            response.setContentType("application/octet-stream; charset=utf-8");
            response.setContentLength((int) file.length());
            String browser = getBrowser(request);
            String disposition = getDisposition(viewFileNm, browser);
            response.setHeader("Content-Disposition", disposition);
            response.setHeader("Content-Transfer-Encoding", "binary");
            OutputStream out = response.getOutputStream();
            FileInputStream fis = null;
            fis = new FileInputStream(file);
            FileCopyUtils.copy(fis, out);
            if (fis != null)
                fis.close();
            out.flush();
            out.close();
        }
    }

    private String getBrowser(HttpServletRequest request) {
        String header = request.getHeader("User-Agent");
        if (header.indexOf("MSIE") > -1 || header.indexOf("Trident") > -1)
            return "MSIE";
        else if (header.indexOf("Chrome") > -1)
            return "Chrome";
        else if (header.indexOf("Opera") > -1)
            return "Opera";
        return "Firefox";
    }

    private String getDisposition(String filename, String browser)
            throws UnsupportedEncodingException {
        String dispositionPrefix = "attachment;filename=";
        String encodedFilename = null;
        if (browser.equals("MSIE")) {
            encodedFilename = URLEncoder.encode(filename, "UTF-8").replaceAll(
                    "\\+", "%20");
        } else if (browser.equals("Firefox")) {
            encodedFilename = "\""
                    + new String(filename.getBytes("UTF-8"), "8859_1") + "\"";
        } else if (browser.equals("Opera")) {
            encodedFilename = "\""
                    + new String(filename.getBytes("UTF-8"), "8859_1") + "\"";
        } else if (browser.equals("Chrome")) {
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < filename.length(); i++) {
                char c = filename.charAt(i);
                if (c > '~') {
                    sb.append(URLEncoder.encode("" + c, "UTF-8"));
                } else {
                    sb.append(c);
                }
            }
            encodedFilename = sb.toString();
        }
        return dispositionPrefix + encodedFilename;
    }

}
```

**메소드**

- public static int copy(InputStream in, OutputStream out)

    지정한 in의 내용을 지정한 out에 복사하고 스트림을 닫는다. 

    리턴값은 copy한 byte 수를 리턴한다. 

참고 URL : 

[FileCopyUtils.copy](https://m.blog.naver.com/gaeean/221739006045)

[FileCopyUtils.copy](http://blog.naver.com/PostView.nhn?blogId=alucard99&logNo=221641168080&categoryNo=44&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search)