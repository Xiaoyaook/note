# Java读写文件

## 读文件的几种方式

### FileInputStream

```Java
static public String readFile(File file) {       

    try {
            FileInputStream fileInputStream = new FileInputStream(file);
            // 把每次读取的内容写入到内存中，然后从内存中获取
            ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len = 0;
            // 只要没读完，不断的读取
            while ((len = fileInputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, len);
            }
            // 得到内存中写入的所有数据
            byte[] data = outputStream.toByteArray();
            fileInputStream.close();
            return new String(data);
            //return new String(data, "GBK");//以GBK（什么编码格式）方式转
        } catch (FileNotFoundException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
```

## 读取小文件的方式
```Java
public static List<String> readFiles(String filePath){
        List<String> textList = new ArrayList<String>();
        try {
            String encoding="utf-8";
            File file=new File(filePath);
            if(file.isFile() && file.exists()){
                InputStreamReader read = new InputStreamReader(
                        new FileInputStream(file),encoding);
                BufferedReader bufferedReader = new BufferedReader(read);
                String lineTxt = null;
                while((lineTxt = bufferedReader.readLine()) != null){
                    textList.add(lineTxt);
                }
                read.close();
            }else{
                System.out.println("文件不存在");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return textList;
    }
```

### 读取大文件（G级以上）

#### 使用 RandomAccessFile

```Java
public static void largeRead(String inFile, String outFile){
        RandomAccessFile read = null;
        try {
            read = new RandomAccessFile(inFile,"r");
            RandomAccessFile writer = new RandomAccessFile(outFile,"rw");
            byte[] b = new byte[200*1024*1024];
            while(read.read(b)!=-1){
 
                writer.write(b);
            }
            writer.close();
            read.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }catch (IOException e){
            e.printStackTrace();
        }
    }

```

#### 使用 LineIterator

```Java
public static void largeRead1(String inFile, String outFile){
 
        try {
            LineIterator li = FileUtils.lineIterator(new File(inFile), "utf-8");
            FileWriter fw = new FileWriter(outFile,true);
            BufferedWriter bw = new BufferedWriter(fw);
            while (li.hasNext()){
                String line = li.nextLine();
                String[] split = line.split("\t");
                if(split.length > 1){
                    //写入outFile
                    bw.write(line);
                    bw.newLine();
                    bw.flush();
                    count++;
                }
            }
            bw.close();
            fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
        }
    }
```

## 写文件

```Java
public static void writeFile(String destinationPath, String line) throws Exception {
        FileWriter fileWriter = new FileWriter(destinationPath,true);
        BufferedWriter writer = new BufferedWriter(fileWriter);
        writer.write(line);
        writer.newLine();
        writer.flush();
 
        writer.close();
        fileWriter.close();
    }
```

## 多个文件合并为一个文件

### 普通方式（小文件）

先读出文件的内容，然后以追加的方式写入新的文件中，结合以上的读和写即可实现

### 快速合并

```Java
public static void fastMerge(String outFile){
        FileChannel outChannel = null;
        try {
            outChannel = new FileOutputStream(outFile).getChannel();
            for (String f : pathList){
                System.out.println("Merge " + f + " into " + outFile);
                FileChannel fc = new FileInputStream(f).getChannel();
                ByteBuffer bb = ByteBuffer.allocate(BUFSIZE);
                while (fc.read(bb) != -1){
                    bb.flip();
                    outChannel.write(bb);
                    bb.clear();
                    count++;
                }
            }
        } catch (IOException e){
            e.printStackTrace();
        }finally {
            try {
                if(outChannel != null){
                    outChannel.close();
                }
            }catch (IOException e){
                e.printStackTrace();
            }
        }
    }

```
