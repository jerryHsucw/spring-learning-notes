# Spring Learning Notes
Ioc 控制反轉 優點:
1.由spring 管理物件 降低 耦合(loose coupling)，沒有spring 會是由各自class new 出物件，而有可以造成改一個物件要到處都改。
2.生命週期由Spring管理
3.方便測試

2-1
@Component: 宣告於class上，代表該物件由spring管理，並且稱呼這種object為bean，Spring 容器名稱為 Bean hpPeinter
@Compoent
public class Hpprint impleements Printer {
}


依賴注入 DI Denpendency Injection
現在有一個print是bean，當teacher需要用print時，也需要將teacher設定@Compent，這兩者是透過spring管理，這兩者的行為便叫做依賴注入

Ioc 將 object 存放在Sping 容器裡面，DI則是讓我們去得得存放在容器的那個object
Bean 存放在Spring 容器裡的object
@Autowird 加在變數上，取得Spring 容器中的bean

2-2
@Qulifier("bean name"):用於當spring 管理的容器都使用同一個interface時，Autowired會不知道要用哪一個bean，所以要特別指定bean的名稱
