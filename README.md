# Halo-boot
基于Spring Boot 的中间件轻量集成方案，与标准的 Spring Boot 工程无缝集成。其在完美集成各个中间件的前提下对用户提供了易用、统一的编程界面。

## 待实现的功能

### 加密
 应用中的数据库连接信息加密
* https://github.com/ulisesbocchio/jasypt-spring-boot
* https://blog.csdn.net/xm526489770/article/details/83412649



### BeanMapper
* https://github.com/42BV/beanmapper-spring-boot-starter
* https://github.com/SoftwareKing/orika-spring-boot-starter

### Spring Boot集成Swagger

* 1.支持多包扫描支持
  使用springfox中的 RequestHandlerSelectors.basePackage(“com.xxx”) 只能支持单个包路径的扫描匹配,如果要想支持多个包路径的匹配我们需要修改springfox里面的代码来支持他，
```java
/**
 * Swagger2配置 <br>
 * 使用springfox中的 RequestHandlerSelectors.basePackage("com.xxx")
 * 只能支持单个包路径的扫描匹配,以下修改支持多包路径匹配。
 *
 */
@Configuration
@EnableSwagger2
public class SwaggerConfig {
	/**
	 * Swagger2创建Docket的Bean
	 * 
	 * @return
	 */
	@Bean
	public Docket createRestApi() {
		return new Docket(DocumentationType.SWAGGER_2).host("127.0.0.1:8080").apiInfo(apiInfo()).select()
				.apis(SwaggerConfig.basePackage("com.zhangqin,com.xxx")).paths(PathSelectors.any()).build();
	}

	/**
	 * Swagger2创建该Api的基本信息
	 * 
	 * @return
	 */
	private ApiInfo apiInfo() {
		return new ApiInfoBuilder().title("黑狗后台API接口文档").description("黑狗后台API接口文档").version("1.0.0").build();
	}

	/**
	 * Predicate that matches RequestHandler with given base package name for the
	 * class of the handler method. This predicate includes all request handlers
	 * matching the provided basePackage
	 *
	 * @param basePackage
	 *            - base package of the classes
	 * @return this
	 */
	public static Predicate<RequestHandler> basePackage(final String basePackage) {
		return new Predicate<RequestHandler>() {

			@Override
			public boolean apply(RequestHandler input) {
				return declaringClass(input).transform(handlerPackage(basePackage)).or(true);
			}
		};
	}

	/**
	 * 处理包路径配置规则,支持多路径扫描匹配以逗号隔开
	 * 
	 * @param basePackage
	 * @return Function
	 */
	private static Function<Class<?>, Boolean> handlerPackage(final String basePackage) {
		return new Function<Class<?>, Boolean>() {

			@Override
			public Boolean apply(Class<?> input) {
				for (String strPackage : basePackage.split(",")) {
					boolean isMatch = input.getPackage().getName().startsWith(strPackage);
					if (isMatch) {
						return true;
					}
				}
				return false;
			}
		};
	}

	/**
	 * @param input
	 * @return Optional
	 */
	@SuppressWarnings("deprecation")
	private static Optional<? extends Class<?>> declaringClass(RequestHandler input) {
		return Optional.fromNullable(input.declaringClass());
	}

}
```

### 全局异常处理
* https://github.com/zhaojun1998/exception-handler-demo
* https://github.com/liushaoming/flylib-boot

## Start统计

[![Stargazers over time](https://starcharts.herokuapp.com/SoftwareKing/halo-boot.svg)](https://starcharts.herokuapp.com/SoftwareKing/halo-boot)
      
