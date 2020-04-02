---
title: 使用POI读取Excel数据踩坑记
date: 2019-05-12 16:08:09
tags: 
- 实际开发过程遇到的问题
---
# Spring Boot项目使用Apache POI读取Excel文件数据
<!-- more -->
****************************
## 第一、导入依赖
```xml
      <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>4.0.1</version>
      </dependency>
```
这里有一个地方需要注意，这个包只能支持xls格式的Excel文件。但是2003年以后，我们现在常用的格式是xlsx格式的文件。所以要正常使用的话还需导入另一个依赖。
```xml
    <dependency>
      <groupId>org.apache.poi</groupId>
      <artifactId>poi-ooxml</artifactId>
      <version>4.0.1</version>
    </dependency>
```
导入这两个依赖以后就可以使用POI相关的API了。

## 第二、使用
直接上代码
```java
@RequestController
@RequestMapping("/file")
public class Controller {
    @PostMapping("/parse.json")
    /**这里直接返回为空（根据需求），关键是里面的代码不是返回什么。
    *用@RequestParam注解接收参数，对象为
    *org.springframework.web.multipart.MultipartFile
    */
    public void parse(@RequestParam MultipartFile file) {
        //检查文件
        checkFile(file);
        //获取workbook对象
        Workbook workbook = getWorkBook(file);
        //获取sheet对象
        Sheet sheet = workbook.getSheetAt(0);

        for (Row row : sheet) {
            if (row.getRowNum() == 0) {
                continue;
            }
            //在这里就可以获取cell对象，从而读取Excel的数据了
            Cell cell = row.getCell(0);
            //cell.getStringCellValue();
            //cell.getNumericCellValue();
            //...等等方法
        }
    }

    public static void checkFile(MultipartFile file) throws IOException {
        //判断文件是否存在
        if (null == file) {
            logger.error("文件不存在！");
            throw new FileNotFoundException("文件不存在！");
        }
        //获得文件名
        String fileName = file.getOriginalFilename();
        //判断文件是否是excel文件
        if (!fileName.endsWith("xls") && !fileName.endsWith("xlsx")) {
            logger.error(fileName + "不是excel文件");
            throw new IOException(fileName + "不是excel文件");
        }
    }

    public static Workbook getWorkbook(MultipartFile file) {
        //获得文件名
        String fileName = file.getOriginalFilename();
        //创建Workbook工作薄对象，表示整个excel
        Workbook workbook = null;
        try {
            //获取excel文件的io流
            InputStream is = file.getInputStream();
            //根据文件后缀名不同(xls和xlsx)获得不同的Workbook实现类对象
            if (fileName.endsWith("xls")) {
                //2003
                workbook = new HSSFWorkbook(is);
            } else if (fileName.endsWith("xlsx")) {
                //2007
                workbook = new XSSFWorkbook(is);
            }
        } catch (IOException e) {
            logger.info(e.getMessage());
        }
        return workbook;
    }
}
```
大致过程是这样，具体有些还需要根据场景
## 第三、遇到的坑
### （一）getLastRowNum()方法获取行数的问题这里是n-1行，而getPhysicalNumberOfRows()获取的是n行。注：标题行会忽略
### （二）注意获取值的时候的格式，这里又有一个坑就是服务器环境系统不同，单元格类型会有差异。解决的想法是，统一写入格式零时存入本地在读取。
### （三）跨服务存取，如果直接传递数据流有可能会丢失。所以最好还是临时存入本地，再获取数据流。




