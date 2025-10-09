# Spring Learning Notes
Ioc 控制反轉 優點:
1.由spring 管理物件 降低 耦合(loose coupling)，沒有spring 會是由各自class new 出物件，而有可以造成改一個物件要到處都改。
2.生命週期由Spring管理
3.方便測試

2-1
@Component: 宣告於class上，代表該物件由spring管理，並且稱呼這種object為bean，他的命名方式為
@Compoent
public class Hpprint impleements Printer {
}
Spring 容器名稱為 Bean hpPeinter
