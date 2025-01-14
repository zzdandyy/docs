---
description: '2021-01-29'
---

# day5

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

#### Map

Interface Map

K:键的类型 V:值的类型

实现类HashMap

```java
public class Student {
    private String sid;
    private String name;
    private int age;

    public Student(){}
    public Student(String sid,String name,int age){
        this.sid=sid;
        this.name=name;
        this.age=age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getSid() {
        return sid;
    }

    public void setSid(String sid) {
        this.sid = sid;
    }

    @Override
    public String toString() {
        return "Student{" +
                "sid='" + sid + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
public class HashMapDemo {
    public static void main(String[] args) {
        HashMap<String,Student> hm=new HashMap();
        Student s1=new Student("001","小王",23);
        Student s2=new Student("002","小红",21);
        Student s3=new Student("003","小刚",22);

        hm.put(s1.getSid(),s1);
        hm.put(s2.getSid(),s2);
        hm.put(s3.getSid(),s3);

        Set<String> set=hm.keySet();
        for(String key:set){
            Student s=hm.get(key);
            System.out.println(key+","+s.getName()+","+s.getAge());
        }
        System.out.println("--------");

        Set<Map.Entry<String, Student>> entries = hm.entrySet();
        for (Map.Entry<String, Student> entry : entries) {
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
        }
    }
}
```

**扑克牌**

```java
public class PokerDemo01 {
    public static ArrayList<String> gameInit() {
        ArrayList<String> array = new ArrayList<>();
        String[] colors = {"♦", "♣", "♥", "♠"};
        String[] numbers = {"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};
        for (String color : colors) {
            for (String number : numbers) {
                array.add(color + number);
            }
        }
        array.add("小王");
        array.add("大王");
        return array;
    }

    public static void dealCards(ArrayList<String> array, ArrayList<String> array1, ArrayList<String> array2, ArrayList<String> array3, ArrayList<String> arrayOther) {
        Collections.shuffle(array);
        for (int i = 0; i < array.size() - 3; i++) {
            String poker = array.get(i);
            if (i % 3 == 0) {
                array1.add(poker);
            } else if (i % 3 == 1) {
                array2.add(poker);
            } else {
                array3.add(poker);
            }
        }
        for (int i = array.size() - 3; i < array.size(); i++) {
            String poker = array.get(i);
            arrayOther.add(poker);
        }
    }

    public static void main(String[] args) {

        ArrayList<String> player1 = new ArrayList<>();
        ArrayList<String> player2 = new ArrayList<>();
        ArrayList<String> player3 = new ArrayList<>();
        ArrayList<String> pokerOther3 = new ArrayList<>(); //底牌

        ArrayList<String> pokers = gameInit(); //初始化
        dealCards(pokers, player1, player2, player3, pokerOther3);  //发牌


        System.out.println("玩家1 = " + player1);
        System.out.println("玩家2 = " + player2);
        System.out.println("玩家3 = " + player3);
        System.out.println("底牌 = " + pokerOther3);
    }
}
```

#### IO流

**File**

```java
public class GetNewProject {
    public static void main(String[] args) throws IOException {
        int day = 0;
        Calendar cBase = Calendar.getInstance();
        cBase.set(2021, Calendar.JANUARY, 24); //寒假回家
        Calendar cNow = Calendar.getInstance();
        int mmBase = cBase.get(Calendar.MONTH);
        int ddBase = cBase.get(Calendar.DATE);

        for (int i = 1; i < 40; i++) {
            cNow.add(Calendar.DATE, -1);
            int mm = cNow.get(Calendar.MONTH);
            int dd = cNow.get(Calendar.DATE);
            if (mm == mmBase && dd == ddBase) {
                day = i;
                break;
            }
        }
        String myFile = "src\\day" + day;
        for (int i = 1; i < 100; i++) {
            String fileString = "study" + i;
            File file = new File(myFile, fileString);
            if (file.mkdirs()) {
                System.out.println("666");
                File javaFile = new File(file, "\\StartDemo.java");
                FileOutputStream fos = new FileOutputStream(javaFile);
                String s = "package day" + day + ".study" + i + ";\n" +
                        "\n" +
                        "public class StartDemo {\n" +
                        "}";
                fos.write(s.getBytes(StandardCharsets.UTF_8));
                fos.close();
                break;
            }
        }
    }
}
```

**某一目录 下的所有文件**

```java
public class FileDemo {
    public static void main(String[] args) {
        File file = new File("src");
        GetAllFilePath(file);
    }
    public static void GetAllFilePath(File srcFile){
        File[] files=srcFile.listFiles();
        if(files!=null){
            for (File file : files) {
                if(file.isDirectory()){
                    GetAllFilePath(file);
                } else{
                    System.out.println(file.getAbsolutePath());
                }
            }
        }
    }
}
```

**字节流**

FileOutputStream

FileInputStream

```java
public class FileInputStreamDemo {
    public static void main(String[] args) throws IOException {
        String file="src\\day5\\study5\\FileInputStreamDemo.java";
        FileInputStream fis=new FileInputStream(file);
        FileOutputStream fos=new FileOutputStream(file,true);
        byte[] bytes=new byte[1024];
        int len= fis.read(bytes);
        String s= new String(bytes);
        System.out.println(s);
        fos.write("//读数据和写数据".getBytes(StandardCharsets.UTF_8));
        fos.close();
    }
}
```

复制文件

```java
public class CopyDemo {
    public static void main(String[] args) throws IOException {
        FileInputStream fis=new FileInputStream("src\\day5\\study5\\test.jpg");
        FileOutputStream fos =new FileOutputStream("src\\day5\\study5\\testCopy.jpg");

        byte[] bytes=new byte[1024];
        int len;
        while((len=fis.read(bytes))!=-1){
            fos.write(bytes,0,len);
        }
        fis.close();
        fos.close();
    }
}
```

**字节缓冲流**

BufferOutputStream

BufferInputStream

提高读写效率，即提供缓冲区，真正的读写还是需要io流

```java
public class CopyMp4Demo {
    public static void main(String[] args) throws IOException {
        long startTime = System.currentTimeMillis();

//        复制视频

        method1();   //基本字节流一次读写一个字节，耗时：44349毫秒
        method2();   //基本字节流一次读写一个字节数组，耗时：62毫秒
        method3();   //字节缓冲流一次读写一个字节，耗时：110毫秒
        method4();   //字节缓冲流一次读写一个字节数组，耗时：16毫秒

        long endTime = System.currentTimeMillis();
        System.out.println("耗时：" + (endTime - startTime) + "毫秒");
    }

    private static void method1() throws IOException {
        FileInputStream fis = new FileInputStream("src\\day5\\study6\\test.mp4");
        FileOutputStream fos = new FileOutputStream("src\\day5\\study6\\testMethod1.mp4");

        int by;
        while ((by = fis.read()) != -1) {
            fos.write(by);
        }
        fis.close();
        fos.close();
    }

    private static void method2() throws IOException {
        FileInputStream fis = new FileInputStream("src\\day5\\study6\\test.mp4");
        FileOutputStream fos = new FileOutputStream("src\\day5\\study6\\testMethod2.mp4");

        byte[] bytes = new byte[1024];
        int len;
        while ((len = fis.read(bytes)) != -1) {
            fos.write(bytes, 0, len);
        }
        fis.close();
        fos.close();
    }

    private static void method3() throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("src\\day5\\study6\\test.mp4"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("src\\day5\\study6\\testMethod3.mp4"));

        int by;
        while ((by = bis.read()) != -1) {
            bos.write(by);
        }
        bis.close();
        bos.close();
    }

    private static void method4() throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("src\\day5\\study6\\test.mp4"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("src\\day5\\study6\\testMethod4.mp4"));

        byte[] bytes = new byte[1024];
        int len;
        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes, 0, len);
        }
        bis.close();
        bos.close();
    }
}
```

**字符流**

UTF-8

```java
public class OutputStreamWriterDemo {
    public static void main(String[] args) throws IOException {
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("src\\day5\\study7\\test.txt"));
        InputStreamReader isr = new InputStreamReader(new FileInputStream("src\\day5\\study7\\OutputStreamWriterDemo.java"));
        osw.write("666");
        osw.close();
        char[] chars = new char[1024];
        int len;
        while ((len = isr.read(chars)) != -1) {
            System.out.println(chars);
        }
    }
}
```

**集合中的数据写到文件**

```java
public class ArrayListToTxtDemo {
    public static void main(String[] args) throws IOException {
        ArrayList<String> array = new ArrayList<>();
        array.add("hello");
        array.add("world");
        array.add("hello");
        array.add("java");
        BufferedWriter bw = new BufferedWriter(new FileWriter("src\\day5\\study8\\arrayTxt.txt"));
        for (String s : array) {
            bw.write(s);
            bw.newLine();
            bw.flush();
        }
        bw.close();
    }
}
```

**文件中的数据读到集合**

```java
public class TxtToArrayListDemo {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader("src\\day5\\study8\\arrayTxt.txt"));
        ArrayList<String> array = new ArrayList<>();
        String line;
        while ((line = br.readLine()) != null) {
            array.add(line);
        }
        br.close();
        for (String s : array) {
            System.out.println(s);
        }
    }
}
```

**点名器**

```java
public class CallNameDemo {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader("src\\day5\\study8\\className.txt"));
        ArrayList<String> array = new ArrayList<>();
        String line;
        while ((line = br.readLine()) != null) {
            array.add(line);
        }
        br.close();
        Random random = new Random();
        int getIndex = random.nextInt(array.size());
        System.out.println("幸运者是：" + array.get(getIndex));
    }
}
```

