# Spring Learning Notes
Ioc 控制反轉 優點:
    1.由spring 管理物件 降低 耦合(loose coupling)，沒有spring 會是由各自class new 出物件，而有可以造成改一個物件要到處都改。
    2.生命週期由Spring管理
    3.方便測試

2-1 / 2-2
    @Component: 宣告於class上，代表該物件由spring管理，並且稱呼這種object為bean，Spring 容器名稱為 Bean hpPeinter
    @Compoent
    public class Hpprint impleements Printer {
    }
    依賴注入 DI Denpendency Injection
    現在有一個print是bean，當teacher需要用print時，也需要將teacher設定@Compent，這兩者是透過spring管理，這兩者的行為便叫做依賴注入
    
    Ioc 將 object 存放在Sping 容器裡面，DI則是讓我們去得得存放在容器的那個object
    Bean 存放在Spring 容器裡的object
    @Autowird 加在變數上，取得Spring 容器中的bean

2-3
    @Qulifier("bean name"):用於當spring 管理的容器都使用同一個interface時，Autowired會不知道要用哪一個bean，所以要特別指定bean的名     稱，若有同時使用 例如impleement Printer,但是未用Qulifier,run則會出現以下錯誤訊息
    Consider marking one of the beans as @Primary, updating the consumer to accept multiple beans, or using @Qualifier to identify the bean that should be consumed
    Ensure that your compiler is configured to use the '-parameters' flag.
    You may need to update both your build tool settings as well as your IDE.

2-4
    @configuration / @Compponent :
    spring boot 啟動中 會去檢查class 是否有這兩種@，若是沒有則會忽略這個class。
    @configuration 將class設定為Spring用的class
    @Compponent 將class設定為Spring管理的容器的bean
    @Bean 只能家在帶有@configuration class的方法上，用途為在spring容器中創建一個bean
    //@Qualifier("hpPrinter") //測試有多個xxprinter 去實作 Printer，所以透過指定Bean的名稱，讓Spring知道是哪一個Bean
    @Qualifier("myPrinter") //這是測試當Brother沒有加上@Component時，仍可以透過MyConfigurtaion產生的@Bean直接使用BrotherPrinter

2-5
    @PostConstruct / InitializingBean (interface)
    用於初始化Bean，例如要給xxPrinter 初始化並設定count
    PostConstruct 是在@Compponent創建一個init方法，並初始化
    InitializingBean 是bean實作這個interface，並實作afterProperties()，用於初始化count
    @Component
    public class FujiPrinter implements Printer {
        private int count;
        @PostConstruct
        private void init(){
            count= 5;
        }
        @Override
        public void print(String message) {
            count--;
            System.out.println("This is Fuji:"+message);
            System.out.println("This is Fuji count"+count);
        }
    }

2-6 
    1.Bean的生命週期:創建→初始化→可以使用
    2.創建Bean時，若遇到有依賴其他bean時，則會回頭去進行創建+初始化那個被依賴的bean
    3.不要寫出依賴循環的bean，例如 Bean A autowird Bean B， Bean B autowird Bean A，這樣創建時會互相等待，導致尋換等待。
    4.Spring boot全部的Bean建立好才會完成啟動。

2-7
    Spring boot 分成main 與 test ， main 下分成 java(code) 與 resource。
    Resource內的appliaction.properties 就是 spring的設定檔。
    設定檔的語法：
    kep=value ex.count=5
    #xxx 作為properties的註解
    以下是用來設定抓取properties的參數，@Value("${xxx}")
    @Value("${printer.name}")
    private String printerName;
    如果properties沒有XXXX這個key，可以透過預設值給值
    @Value("${XXXX:Default Printer}") 
    
    appliaction.properties or appliaction.yml 這兩種都是設定檔，但是其中語法不同。
    
    3-1 AOP 切面導向程式設計 : 讓某些重複執功能，由切面程式統一執行，例如:要執行的紀錄某些功能的開始執行時間與結束執行時間。
    3-2 AOP 相關@說明
    @Aspect: 要同時加上@Component一起使用
    @Beforce :要指定pointcut(切入點):指的是某一個方法之前要執行該有這個@的方法
    @After:要指定pointcut(切入點):指的是某一個方法之後要執行該有這個@的方法
    @Around:再切入點的方法前後都會執行
    
    Pointcut路徑說明:
     @Around("execution(* com.test.demo.HpPrinter.*(..))") :切入點為com.test.deom package裡的HpPinter class 的所有方法
     @Around("execution(* com.test.demo.*(..))") :切入點為com.test.deom package裡的class 的所有方法
     
    spring Aop的發展:
    權限驗證 → Spring Security 處理
    統一的exception處理 → @ControllerAdvice 處理
    log紀錄
    要看公司的使用方式，有需要再來查即可。

4-2 http協議:
    負責規定資料的傳輸格式，讓前端與後端能夠有效地進行資料溝通。
    可以分成request and response兩部分    

5-3 JDBC連線資料庫設定
    因為不是用IntelliJ內建的資料連線，所以這邊作法調整為
    1.先安裝docker 與 設定 mysql連線
    2.安裝dbevar
    3.prom.xml設定
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.22</version>
    </dependency>
    5.appliaction.properties設定連線資訊
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.datasource.url=jdbc:mysql://localhost:3307/springboot_db?allowPublicKeyRetrieval=true&useSSL=false&serverTimezone=Asia/Taipei&characterEncoding=utf-8
    spring.datasource.username=xx
    spring.datasource.password=xx

8-1 Maven library管理
    1.管理spring boot project 可以使用那些功能、透過prom.xml設定
    2.<dependency> 放入自己要的libary </dependency>
    3.可以到https://mvnrepository.com/artifact/mysql/mysql-connector-java 查詢自己要的library
    4.group id 如果是spring boot starter 不可指定version的值，而是統一由<groupId>org.springframework.boot</groupId>決定
    groupId:公司名稱
    artifactId:功能的名字
    version 為該功能的版本
    scope 若為 test，只能在單元測試內使用
    exclusions 設定在裡面的libray不需加載
    ex.
    <dependency>
        <groupId>org.springframework.boot</groupId> 
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>
    5. Maven Repository 
    儲存sping boot project 用到的libaray
    儲放位置分成 local or remote，先找loacl是否有該jar檔，如果沒有就去remote找。
    Jar 預設路徑為 ~/.m2/resposity內:C:\Users\user\.m2\repository，若是沒有這個intelj會自動用預設的倉庫，若需要自己新增remote位置可以自己創該檔案。

8-2 Maven Project管理
    用途:打包Spring boot project的程式
    常用指令：
    Maven 指令分成3條生命週期(life cycle)，每一條生命週期比齒不互相干擾，在同一條生命周期，後面的指令會執行前面的所有指令. ex 當執行package指令:實際上會先執行complier > test > package
    1.clean生命週期:clean 刪除target資料夾/ target資料室spring 運行後的結果(常用)
    2.default生命週期:
    complier 編譯spring boot程式
    test 運行單元測試
    package 打包成,jar存放在target資料夾內(常用)
    install 將jar存放到local倉庫
    deploy 將.jar檔上傳到remote倉庫
    
    package後的jar檔會產生於此＂C:\Users\user\IdeaProjects\demo_hahow_reviiew_1_4\demo\target＂，這邊可以測試執行。
    將demo-0.0.1-SNAPSHOT.jar放到指定路徑
    然後cmd到指定路徑輸入:C:\Users\user\Desktop>java -jar demo-0.0.1-SNAPSHOT.jar
    即可執行該jar檔
    
    local 端 打包jar的設定
    <groupId>com.test</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    -SNAPSHOT(不穩定版本):可以無限次上傳到remote倉庫 (jar的檔名會含有-SNAPSHOT)
    -RELEASE(穩定版本):只能上傳一次 且 不能覆蓋(jar的檔名不會有-SNAPSHOT)、改成<version>0.0.1</version>，這樣生成就是沒有snashot的jar檔

8-8 Thymeleaf
    springboot 的一種前端模板引擎
    spring-boot-starter-thymeleaf
    增加了一個thymeleaf demo
    
    8-9 Bean 注入的三種方式
    1.field injection 變數注入
      優點:簡單好上手
      缺點:容易注入過多的bean進來
    2.constructor injection 建構子注入
      優點:容易釐清依賴關係、測試、變數可以為final
      缺點:寫起來較為冗長(可用lombok簡化)
    3.setter injection setter 注入
      chatgpt回復
      優點：
      可選依賴很方便（不需要注入時可不呼叫 setter 或將參數設為 optional）。
      比 field injection 更容易被測試（可以在測試時呼叫 setter 注入 mock）。
      適合需要在建構後才設定或可變的依賴（runtime 可修改）。
      缺點：
      不能強制在物件建立時提供依賴（無法保證依賴一定被設定），較不利於不變性（final 無法使用）。
      可能導致物件在未完全初始化前就被使用（需要注意初始化順序）。
      程式碼較多（多個 setter 會明顯增加樣板），且不如 constructor 注入在閱讀上能立刻看出必需的依賴。

8-10 springboot 2 > 3的差異
    1. jdk 只能用17
    2. java ee 遷移到 jakata ee，也就是javax.* 轉移到 jakata.*
    3. 持續優化GraalVM的技術，例如改善jar打包的容量
    
9-1 9-2 : intellij 的 debug用法

9-3 intellij 實用技巧
    1. 萬用鍵 alt + enter
    2. ctrl + 左鍵 跳到指定的方法 / 點選I符號可以跳到interface
    3. 全局搜尋：windows > ctrl+shift+F
    4. 延伸2. 搭配ctrl + alt + 鍵盤的← →，跳回上一層方法
    5. 下面選的endpoint功能，會呈現該url mapping，點選會直接回到Mapping的class
    6. class 檔案按滑鼠右鍵，選擇split right 可以呈現左右視窗
    7. ctrl + alt + o 可以清除該Class未使用的import
    8. ctrl + alt + l 可以美化排版
    9. 連按兩下shift 打開一個搜尋視窗，可以選擇要查詢class or file or 其他
    10. ctrl + e 查詢曾經開啟過的檔案
    11. 滑鼠右鍵 選擇 generate > getter and setter
    12. 依次選取多行 alt 往下拉選 (有點類似utral edit 的區塊模式多筆輸入)
    13. 載入多個專案：flie > new > moduel from exsiting source，選擇你要的spring boot 專案，點選pom檔
    14. 右上角齒輪，裡面有一個plugin，可以使用外掛程式

9-4 lambok
    1.確認plugin是否有lombok，沒有的話要安裝
    2.prom.xml將dependency 加入
            <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.34</version>
            <scope>provided</scope>
        </dependency>
    








