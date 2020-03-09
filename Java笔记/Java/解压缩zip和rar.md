解压缩rar 使用了junrar，解压缩zip使用了 zipInputStream

```java
package com.netease.channelpublish.service.autopublish;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

import com.github.junrar.Junrar;
import com.github.junrar.exception.RarException;

public class DeCompressUtil {
    /**
     * 解压zip格式压缩包
     */
    static void unzip(String path, String target) throws IOException {
        File targetfolder = new File(target);
        ZipInputStream zi = new ZipInputStream(new FileInputStream(path));
        ZipEntry ze;
        FileOutputStream fo;
        byte[] buff = new byte[1024];
        int len;
        while ((ze = zi.getNextEntry()) != null) {
            File _file = new File(targetfolder, ze.getName());
            if (!_file.getParentFile().exists()) _file.getParentFile().mkdirs();
            if (ze.isDirectory()) {
                _file.mkdir();
            } else {
                fo = new FileOutputStream(_file);
                while ((len = zi.read(buff)) > 0) fo.write(buff, 0, len);
                fo.close();
            }
            zi.closeEntry();
        }
        zi.close();
    }

    /**
     * 解压缩
     */
    public static void deCompress(String sourceFile, String destDir) throws Exception {
        //保证文件夹路径最后是"/"或者"\"
        char lastChar = destDir.charAt(destDir.length() - 1);
        if (lastChar != '/' && lastChar != '\\') {
            destDir += File.separator;
        }
        //根据类型，进行相应的解压缩
        String type = sourceFile.substring(sourceFile.lastIndexOf(".") + 1);
        if (type.equals("zip")) {
            DeCompressUtil.unzip(sourceFile, destDir);
        } else if (type.equals("rar")) {
            DeCompressUtil.unrar(sourceFile, destDir);
        } else {
            throw new Exception("只支持zip和rar格式的压缩包！");
        }
    }

    private static void unrar(String sourceFile, String destDir) throws IOException, RarException {
        final File rar = new File(sourceFile);
        final File dstFile = new File(destDir);
        if(!dstFile.exists()) {
            dstFile.mkdirs();
        }
        Junrar.extract(rar, dstFile);
    }
}


```

