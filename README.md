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
@Qulifier("bean name"):用於當spring 管理的容器都使用同一個interface時，Autowired會不知道要用哪一個bean，所以要特別指定bean的名稱
若有同時使用 例如impleement Printer,但是未用Qulifier,run則會出現以下錯誤訊息
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
