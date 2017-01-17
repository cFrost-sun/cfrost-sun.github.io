---
layout: default
title: 可以获取Response Data的HttpServletResponseWrapper
---
## {{ page.title }}
> 目前只是一个初稿，尚未经过严密测试。

```
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.UnsupportedEncodingException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpServletResponseWrapper;

public class DataAccessableResponseWrapper extends HttpServletResponseWrapper {

    private ByteArrayOutputStream buffer;
    private ServletOutputStream out;
    private PrintWriter writer;

    public DataAccessableResponseWrapper(HttpServletResponse response) throws IOException {
        super(response);
        buffer = new ByteArrayOutputStream();// 真正存储数据的流
        out = new BufferAccessableOutputStream(buffer);
        writer = new PrintWriter(new OutputStreamWriter(buffer, this.getCharacterEncoding()));
    }

    @Override
    public ServletOutputStream getOutputStream() throws IOException {
        return out;
    }

    @Override
    public PrintWriter getWriter() throws UnsupportedEncodingException {
        return writer;
    }

    @Override
    public void flushBuffer() throws IOException {
        if (out != null) {
            out.flush();
        }
        if (writer != null) {
            writer.flush();
        }
    }

    @Override
    public void reset() {
        buffer.reset();
    }

    /* 将out、writer中的数据强制输出到buffer里面，否则取不到数据 */
    public byte[] getResponseData() throws IOException {
        flushBuffer();
        return buffer.toByteArray();
    }

    /* 对ServletOutputStream进行包装 */
    private class BufferAccessableOutputStream extends ServletOutputStream {
        private ByteArrayOutputStream byteArrayOutputStream;

        public BufferAccessableOutputStream(ByteArrayOutputStream stream) throws IOException {
            super();
            byteArrayOutputStream = stream;
        }

        @Override
        public void write(int b) throws IOException {
            byteArrayOutputStream.write(b);
        }
    }

    /* 销毁对象(是否有此必要?) */
    @Override
    public void finalize() throws Throwable {
        super.finalize();
        buffer.close();
        out.close();
        writer.close();
    }
}
```

{{ page.date | date_to_string }}