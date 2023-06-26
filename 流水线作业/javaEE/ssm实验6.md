导入相关依赖

```xml
```

项目结构

- Config： 
  - 拦截器配置
  - AOP日志切面
  - JackSonconfig
  - 全局日志管理
- Controller
  - LoginControler： 登录业务接口
  - TBookController: 图书操作接口
- core
  - ResponseResult： 统一的JSON返回结果
- dao:
  - tBookMapper： 
- entity
- init: 项目初始化时候的操作
- serivce： 业务接口
  - serviceIMPL
- sqlMapper: mapper的xml文件
- utils: 通用的工具类
- views： 前端页面

![image-20230615160951552](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230615160951552.png)

实体映射数据库

```java
@ApiModel
@Data
@EqualsAndHashCode(callSuper = false)
@AllArgsConstructor
@NoArgsConstructor
@JsonIgnoreProperties(ignoreUnknown = true)
public class TBookEntity extends CommonEntity implements Serializable {
    private static final long serialVersionUID = 1L;
   @ApiModelProperty(value = "id", name = "id")
    private Integer id;
   @ApiModelProperty(value = "book_name", name = "bookName")
    private String bookName;
   @ApiModelProperty(value = "book_author", name = "bookAuthor")
    private String bookAuthor;
   @ApiModelProperty(value = "book_price", name = "bookPrice")
    private BigDecimal bookPrice;
   @ApiModelProperty(value = "book_public_time", name = "bookPublicTime")
    private Date bookPublicTime;
   @ApiModelProperty(value = "book_intro", name = "bookIntro")
    private String bookIntro;
   @ApiModelProperty(value = "book_addr", name = "bookAddr")
    private String bookAddr;
   @ApiModelProperty(value = "book_num", name = "bookNum")
    private Integer bookNum;
   @ApiModelProperty(value = "state", name = "state")
    private Integer state;
   @ApiModelProperty(value = "create_time", name = "createTime")
    private Date createTime;
}
```

