title: "JSH系统工程简介以及相关常用逻辑方法功能"

author: me

date: 2018-08-16  13:07:06 +0800

update: 2018-08-16  13:07:06 +0800

tags:

- 技术笔记

preview: JSH系统工程简介以及相关常用逻辑方法功能

hide: true

------



### JSH系统工程相关笔记

#### 一、JSH系统工程相关目录（开发常用）

​	

|           工程名           |                备注                | 常用逻辑 |
| :------------------------: | :--------------------------------: | :------: |
|    ylh-cloud-goods-api     |   易理货Spring Cloud商品服务API    |          |
|  ylh-cloud-goods-provider  | 易理货Spring Cloud商品服务provider |          |
|   ylh-service-goods-api    |         易理货商品服务API          |          |
| ylh-service-goods-provider |       易理货商品服务provider       |          |
|      ylh-v2-web-goods      |     易理货二期商品模块web工程      |          |

#### 二、git操作

1. 切换分支-切到需要的分支，master（预生产上线）、dev（测试上线）

git co master/dev

2. 在当前分支下，更新项目

git pull

3. 拉取新的分支，进行准备开发

git co -b user/(任务T/事项M+了然bug号) 最新tag

4. 开发完毕，进行所有添加、提交本地

git add -A

git commit -m "任务T/事项M+了然bug号:完整的了然标题"

5. 提交远程change

git push origin HEAD:refs/for/(master/dev) ---根据具体情况提交



#### 三、开发常用

##### 1.常用小方法

```
StringUtils.isEmpty(storeCheckDto.getCodeOrName())//判空方法
```

```
BeanUtils.copyProperties(s, param);//实体间属性复制（source，target）
```

```
StringUtils.upperCase//转大写
```

```
@CustomerPath//描述controller，用来说明伞下店
```

```
private Logger logger = LoggerFactory.getLogger(getClass());//通用日志获取类
```

```
Long memberId = LoginContextHelper.getShopMemberId();//理货商id
Long customerId = LoginContextHelper.getUserMemberId();//伞下店客户id
```

```
String date = LocalDate.now().format(DateTimeFormatter.ofPattern("yyyy-MM"));//当月时间
```

```
List newList = monthModelTargetResult.getResult().stream()//实现流式分页
    .skip(pager.getPageOffset())
    .limit(monthModelTrackParam.getRows()).collect(toList());
pager.setRecords(newList);
pager.setTotalCount(monthModelTargetResult.getResult().size());
pager.setSuccess(true);
```

```
CheckEmptyUtil.isNotEmpty(）//检查是否为不空
```

```
if (StringUtils.isNotBlank(skuId)) {//判断字符串非空
  param.put(SKU_ID, Long.valueOf(skuId));
}
```

```
//获取时间符合要求的促销
redisDtoList = redisDtoList.stream().filter(dto -> dto.getMarketingEndTime() != null
    && (dto.getMarketingEndTime().getTime() - System.currentTimeMillis() >= 0))
    .collect(Collectors.toList());
```

```
//排序
Collections.sort(redisDtoList, Comparator
    .comparing(MarketingCampaignRedisDto::getMarketingEndTime));
```

```
List<MarketingCampaignRedisDto> list =//redis获取元素
    JSONObject.parseArray(redis.opsForValue().get(key), MarketingCampaignRedisDto.class);
redisDtoList.addAll(list);//集合添加集合
```

```
setScale(2,BigDecimal.ROUND_HALF_UP)//保留两位小数，四舍五入
```

##### 2.开发规范

```
/**
 * 会员目标型号追踪列表.   // . 代码检查格式必须 
 *                       //空行代码检查必须
 * @param  monthModelTrackParam monthModelTrackParam //javadoc 非空标志
 * @return
 */
```

##### 3.常用实现

```
@RequestMapping("/list")
public ModelAndView customerList(MonthModelTrackParam monthModelTrackParam) {
  ModelAndView mav = new ModelAndView();
  mav.setViewName("salestarget/modelTargetTrackCustomer");
  return mav;
}
/**
视图层，适用于列表和条件查询通用情况
跳转链接，自动执行查询方法
*/
```

```
@RequestMapping("/query")
@ResponseBody
public ExecuteResult<Pager<MmCustomerTrackDto>> customerQuery(
    @RequestBody  MonthModelTrackParam monthModelTrackParam)
    //最好的前后端实现返回方法
    
    
    Pager pager = new Pager<>();
    pager.setRows(monthModelTrackParam.getRows());
    pager.setPage(monthModelTrackParam.getPage() == 0 ? 1 : monthModelTrackParam.getPage());
    
    
    pager.setRecords(monthModelTargetResult.getResult());
    pager.setTotalCount(monthModelTargetResult.getResult().size());
    pager.setSuccess(true);
    
    // 返回信息
    ExecuteResult<Pager<MmCustomerTrackDto>> executeResult = new ExecuteResult<>();
    executeResult.setResult(pager);
    executeResult.setSuccess(true);
    return executeResult;
```

```
//Excel导出功能实现
/**
 * 会员目标型号导出.
 *
 * @param  monthModelTrackParam response
 * @return
 */
@RequestMapping("/import")
public void customerImport(HttpServletResponse response,
                           MonthModelTrackParam monthModelTrackParam) throws IOException {

  Long memberId = LoginContextHelper.getShopMemberId();//理货商id
  Long customerId = LoginContextHelper.getUserMemberId();//伞下店客户id
  monthModelTrackParam.setMemberId(memberId);//设置理货商
  monthModelTrackParam.setCustomerId(customerId);//设置伞下店
  if (!"".equals(monthModelTrackParam.getStartMonth())
      || monthModelTrackParam.getStartMonth() != null) {
    monthModelTrackParam.setStartMonth(monthModelTrackParam.getStartMonth().replace("-",""));
  }
  if (!"".equals(monthModelTrackParam.getEndMonth())
      || monthModelTrackParam.getEndMonth() != null) {
    monthModelTrackParam.setEndMonth(monthModelTrackParam.getEndMonth().replace("-",""));
  }

  // 设置单元格内容
  List<String> list = new ArrayList<>();
  list.add("月份");
  list.add("产业");
  list.add("产品组");
  list.add("商品编码");
  list.add("商品型号");
  list.add("目标");
  list.add("实际");
  list.add("承接占比");
  // 创建excel的文档对象
  XSSFWorkbook wkb = new XSSFWorkbook();
  // 建立新的sheet对象（excel的表单）
  Sheet sheet = wkb.createSheet(EXECL_TITLE);
  Row row = sheet.createRow(0);
  CellStyle cellStyle = wkb.createCellStyle();
  cellStyle.setAlignment(HorizontalAlignment.CENTER); // 居中
  cellStyle.setVerticalAlignment(VerticalAlignment.CENTER); // 垂直

  Row row1 = sheet.createRow(1);
  int count = 0;
  for (int i = 0; i < list.size(); i++) {

    if (i >= 7) {
      Cell cell1 = row.createCell(i + 2);
      sheet.addMergedRegion(new CellRangeAddress(0, 1, i + 2, i + 2));
      cell1.setCellValue(list.get(i));
      cell1.setCellStyle(cellStyle);
    } else if (i < 7 && i > 4) {
      Cell cell1 = row.createCell(5 + count * 2);
      sheet.addMergedRegion(new CellRangeAddress(0, 0, 5 + count * 2, 6 + count * 2));
      cell1.setCellValue(list.get(i));
      cell1.setCellStyle(cellStyle);
      // 第二行
      row1.createCell(5 + count * 2).setCellValue("数量（台）");
      row1.createCell(6 + count * 2).setCellValue("金额（万元）");
      count++;
    } else {
      Cell cell1 = row.createCell(i);
      sheet.addMergedRegion(new CellRangeAddress(0, 1, i, i));
      cell1.setCellValue(list.get(i));
      cell1.setCellStyle(cellStyle);
    }
  }
  //伞下店目标型号查询
  ExecuteResult<List<MmCustomerTrackDto>> monthModelTargetResult =
      cusMonthModelTargetService.findCusMonthModelTarget(monthModelTrackParam);

  if (monthModelTargetResult.isSuccess()) {
    for (MmCustomerTrackDto dto: monthModelTargetResult.getResult()) {
      dto.setTargetAmount(dto.getTargetAmount().divide(BigDecimal.valueOf(10000)));
      dto.setActualAmount(dto.getActualAmount().divide(BigDecimal.valueOf(10000)));
    }
  }
  for (MmCustomerTrackDto customerTrackDto : monthModelTargetResult.getResult()) {
    Row row2 = sheet.createRow(2);
    row2.createCell(0).setCellValue(customerTrackDto.getMonthTime());
    row2.createCell(1).setCellValue(customerTrackDto.getClassifyName());
    row2.createCell(2).setCellValue(customerTrackDto.getProductGroupName());
    row2.createCell(3).setCellValue(customerTrackDto.getProductCode());
    row2.createCell(4).setCellValue(customerTrackDto.getItemModel());
    row2.createCell(5).setCellValue(customerTrackDto.getTargetNum());
    row2.createCell(6).setCellValue(customerTrackDto.getTargetAmount().doubleValue());
    row2.createCell(7).setCellValue(customerTrackDto.getActualNum());
    row2.createCell(8).setCellValue(customerTrackDto.getActualAmount().doubleValue());
    row2.createCell(9).setCellValue(customerTrackDto.getActualAmount()
                                        .doubleValue() / customerTrackDto.getTargetAmount()
        .doubleValue() * 100 + "%");
  }
  // 输出Excel文件
  String fileName = new String(EXECL_TITLE.getBytes(), CHART_ISO);
  response.reset();
  response.setHeader(CONTENT_DISPOSTION, ATTACHMENT_FILENAME + fileName + XLSX);
  response.setContentType(APPLICATION_MSEXCEL);
  OutputStream output = response.getOutputStream();
  wkb.write(output);
  output.close();
  wkb.close();
}
```

```
<dubbo:reference interface="cn.gooday.ylh.service.salestarget.api.service.CusMonthModelTargetService"
                 id="cusMonthModelTargetService" version="${jsh.dubbo.customer.order.version:1.0}"/>
                 //消费者注入服务，要进行配置
```

```
<dubbo:service interface="cn.gooday.ylh.service.salestarget.api.service.CusMonthModelTargetService"
               ref="cusMonthModelTargetService" version="${jsh.dubbo.provider.order.version:1.0}"/>
```

```
List<MmCustomerTrackDto> list = cusMonthModelTargetMapper
    .findCusMonthModelTarget(monthModelTrackParam);
    //mapper,查询方式
```

```
List<MmCustomerTrackDto> findCusMonthModelTarget(
    @Param("param") MonthModelTrackParam monthModelTrackParam);
    //param，可进行引用，#{param.attr}
    //可多种形式，mapper.xml中，可以foreach
```

```
<resultMap id="RM_TargetMonthModel"
           type="cn.gooday.ylh.service.salestarget.api.dto.MmCustomerTrackDto">
           //resultMap定义结果
```

##### 4.常用sql

```
//常见的sql条件
sum( IFNULL( tmms.item_num, 0 )) AS item_num,
<sql id="where_condition">
  <where>
    and asd_.status != 2
    <if test="param.customerId !=null ">
      AND tmms.customer_id = #{param.customerId}
    </if>
    <if test="param.memberId != null">
      AND tmms.member_id = #{param.memberId}
    </if>
    <if test="param.targetClassifyId != null ">
      AND tmms.target_classify_id = #{param.targetClassifyId}
    </if>
    <if test="param.classifyName != null and param.classifyName != ''">
      and target_classify_.classify_name = #{param.classifyName}
    </if>
    <if test="param.classifyCode != null and param.classifyCode != ''">
      and target_classify_.classify_code = #{param.classifyCode}
    </if>
    <if test="param.startMonth != null and param.startMonth != ''">
      <![CDATA[ and tmms.month_time >= #{param.startMonth} ]]>
    </if>
    <if test="param.endMonth != null and param.endMonth != ''">
      <![CDATA[ and tmms.month_time <= #{param.endMonth} ]]>
    </if>
    <if test="param.productGroupName != null and param.productGroupName != ''">
      <![CDATA[ AND tmms.product_group_name like CONCAT("%",#{param.productGroupName},"%") ]]>
    </if>
    <if test="param.itemNameAndCode != null and param.itemNameAndCode != ''">
      <![CDATA[ AND
    (tmms.product_code like CONCAT("%",#{param.itemNameAndCode},"%")
    or tmms.item_model like CONCAT("%",#{param.itemNameAndCode},"%")
    or tmms.item_name like CONCAT("%",#{param.itemNameAndCode},"%"))]]>
    </if>
    //循环
    <if test="entity.skuIds != null and entity.skuIds.size()>0">
      <![CDATA[
            AND a.sku_id  in
          ]]>
      <foreach collection="entity.skuIds" item="skuId"  open="(" separator="," close=")">
        #{skuId}
      </foreach>
    GROUP BY
    tmms.member_id,
    tmms.customer_id,
    tmms.classify_code
    limit #page.pageOffSet,#page.rows//sql分页
  </where>
```

```
添加理货商的菜单：

insert

	into

		sys_menu ( NAME,

		url,

		type,

		buss_type,

		pid,

		`status`,

		sort,

		app_id ) select

			"目标型号达成追踪",

			"/shopmanager/target-month-model-track/list",

			1,

			1,

			1039,

			1,

			321,

			1;

 insert

	into

		sys_permission ( CODE,

		NAME,

		STATUS,

		sort,

		app )

	values ( 'target-month-model-track',

	'目标型号达成追踪',

	1,

	22,

	'YLH' );

 insert

	into

		sys_permission_menu ( permission_id,

		menu_id ) select

			(

			select

				id

			from

				sys_permission

			where

				`name` = "目标型号达成追踪" ) permission_id,

			id

		from

			sys_menu

		where

			`name` = "目标型号达成追踪";

 insert

	into

		sys_permission_show_group_rela ( group_id,

		permission_id ) select

			8,

			t.id

		from

			sys_permission t

		where

			t.name = "目标型号达成追踪";

 添加伞下店的菜单 insert

	into

		sys_menu ( NAME,

		url,

		type,

		buss_type,

		pid,

		`status`,

		sort,

		app_id ) select

			"目标型号达成追踪",

			"/shopmanager/target-month-model-track-cus/list",

			1,

			0,

			1073,

			1,

			541,

			1;

```

##### 5.redis操作-StringRedisTemplate

```
stringRedisTemplate.opsForValue().set("test", "100",60*10,TimeUnit.SECONDS);//向redis里存入数据和设置缓存时间  
```

```
stringRedisTemplate.boundValueOps("test").increment(-1);//val做-1操作  
```

```
stringRedisTemplate.opsForValue().get("test")//根据key获取缓存中的val  
```

```
stringRedisTemplate.boundValueOps("test").increment(1);//val +1  
```

```
stringRedisTemplate.getExpire("test")//根据key获取过期时间  
```

```
stringRedisTemplate.getExpire("test",TimeUnit.SECONDS)//根据key获取过期时间并换算成指定单位  
```

```
stringRedisTemplate.delete("test");//根据key删除缓存  
```

```
stringRedisTemplate.hasKey("546545");//检查key是否存在，返回boolean值  
```

```
stringRedisTemplate.opsForSet().add("red_123", "1","2","3");//向指定key中存放set集合  
```

```
stringRedisTemplate.expire("red_123",1000 , TimeUnit.MILLISECONDS);//设置过期时间  
```

```
stringRedisTemplate.opsForSet().isMember("red_123", "1")//根据key查看集合中是否存在指定数据  
```

```
stringRedisTemplate.opsForSet().members("red_123");//根据key获取set集合  
```



#### 四、前端工程启动测试方法

1. 启动相关前端项目

   进入相关前端工程，git bash here

   . app-run.sh

   启动成功

   问题：若出现需要登陆之后操作，则配置

   ```
   jsh.shiro.cache.key.prefix=ylh_cache_dev
   jsh.shiro.session.key.prefix=ylh_session_dev
   ```

2. 浏览器访问 dev.yilihuo.com/index/login

3. debug测试

4. host配置

   12.168.3.15     3bc1fda3.ngrok.io
   127.0.0.1     dev.yilihuo.com

#### 五、数据库相关地址记录

服务器地址：12.168.3.71

数据库：ylh_goods

用户名：ylh_user

密码：ylhuser991ui

#### 六、表情况

​	表final_item_channel_group

| 列名                | 注释             | 关联表                | 类型     | 长度 | 可为空 | 码值                                                         | 知识记录                                                     |
| ------------------- | ---------------- | --------------------- | -------- | ---- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| id                  | 主键             |                       | int      | 11   | 否     |                                                              | 1.spu：标准产品单位                                          |
| member_id           | 用户memberId     | final_channel_member  | int      | 11   | 否     |                                                              | 2.sku：库存量单位                                            |
| item_id             | 商品id           | final_channel_item    | int      | 11   | 否     |                                                              | 3.一个SKU可以对应多个SPU，简单的说： SPU就是一个iPhone6s, SKU就是银色iPhone6s、粉色iPhone6s |
| type                | 类型             | final_channel_type    | int      | 2    | 否     | 类型1 产品库商品 2 总部推荐3 中心推荐4 理货商推荐5 主动引用的信息6 商家自建 | 4.海尔目前是1:1                                              |
| target_id           | 展示终端         |                       | int      | 11   | 是     | 展示终端 1海尔2统帅3海尔/统帅                                |                                                              |
| channel_group_id    | 频道分组ID       | final_channel_channel | int      | 11   | 是     |                                                              |                                                              |
| channel_group_name  | 频道分组名称     |                       | varchar  | 255  | 是     |                                                              |                                                              |
| channel_group_order | 分组顺序         |                       | int      | 6    | 是     |                                                              |                                                              |
| start_time          | 活动频道开始时间 |                       | datetime |      | 是     |                                                              |                                                              |
| end_time            | 活动频道结束时间 |                       | datetime |      | 是     |                                                              |                                                              |
| create_time         | 创建日期         |                       | datetime |      | 否     |                                                              |                                                              |
| update_time         | 修改日期         |                       | datetime |      | 是     |                                                              |                                                              |
| create_user_id      | 创建人           |                       | int      | 11   | 是     |                                                              |                                                              |
| update_user_id      | 修改人           |                       | int      | 11   | 是     |                                                              |                                                              |
| version             | 版本控制         |                       | int      | 11   | 是     |                                                              |                                                              |

​	表final_item_list

| 列名                    | 注释               | 关联表               | 类型     | 长度   | 可为空 | 码值                                                         | 知识记录 |
| ----------------------- | ------------------ | -------------------- | -------- | ------ | ------ | ------------------------------------------------------------ | -------- |
| id                      | 主键               |                      | int      | 11     | 否     |                                                              |          |
| member_id               | 用户memberId       | final_channel_member | int      | 11     | 否     |                                                              |          |
| item_id                 | 商品id             | final_channel_item   | int      | 11     | 否     |                                                              |          |
| type                    | 类型               | final_channel_type   | int      | 11     | 否     | 类型 各种信息类型中级别 最高的 1 产品库2 总部推荐3 中心推荐4 理货商推荐5 主动引用的信息6 商家自建 |          |
| member_from             | 来源人             |                      | int      | 11     | 是     | 如果是总部/中心推荐，那么为-1， 理货商推荐来源memberId       |          |
| type_item               | 产品类型（spu）    |                      | int      | 11     | 是     | 类型1 产品库6 商家自建                                       |          |
| brand_id                | 品牌id             |                      | int      | 11     | 是     |                                                              |          |
| brand_en                | 品牌code           |                      | varchar  | 40     | 是     |                                                              |          |
| brand_type_id           | 品牌类型           |                      | int      | 11     | 是     |                                                              |          |
| brand_name              | 品牌名称           |                      | varchar  | 100    | 是     |                                                              |          |
| cid                     | 商品类目id         |                      | int      | 11     | 是     |                                                              |          |
| cid_name                | 商品类目名称       |                      | varchar  | 100    | 是     |                                                              |          |
| product_group_code      | 产品组编码         |                      | varchar  | 100    | 是     |                                                              |          |
| product_group_name      | 产品组名称         |                      | varchar  | 100    | 是     |                                                              |          |
| item_unit               | 商品单位;计量单位  |                      | varchar  | 10     | 是     |                                                              |          |
| type_item_sku           | 单品类型（sku）    |                      | int      | 2      | 是     | 类型1 产品库6 商家自建                                       |          |
| item_sku_id             | 单品id             |                      | int      | 11     | 是     |                                                              |          |
| product_code            | 编码               |                      | varchar  | 100    | 是     |                                                              |          |
| item_model              | 型号               |                      | varchar  | 100    | 是     |                                                              |          |
| type_member_rela        | 关联会员类型       |                      | int      | 2      | 是     | 2 总部推荐3 中心推荐4 理货商推荐5 主动引用的信息6 商家自建   |          |
| target_id               | 展示终端           |                      | int      | 11     | 是     | 1海尔2统帅3海尔/统帅                                         |          |
| item_status             | 商品状态           |                      | int      | 11     | 是     | 1-草稿,3-启用,30-停用,40-删除                                |          |
| item_member_rela_id     | 关系管理信息表id   |                      | int      | 11     | 是     | 可能为null，null可以说明这个商品是推荐商品                   |          |
| item_status_tv          | 云店上下架状态     |                      | int      | 11     | 是     | 10 上架 20 下架 null是无上下架权限                           |          |
| item_status_wei         | 微商城上下架状态   |                      | int      | 11     | 是     | 10 上架 20 下架 null是无上下架权限                           |          |
| item_status_ylh         | 易理货上下架状态   |                      | int      | 11     | 是     | 10 上架 20 下架 null是无上下架权限                           |          |
| ylh_upload_time         | 易理货上架时间     |                      | datetime |        | 是     |                                                              |          |
| type_info_part_basic    |                    |                      | int      | 2      | 是     | 类型1 产品库2 总部推荐3 中心推荐4 理货商推荐5 主动引用的信息 |          |
| item_name               | 商品名称           |                      | varchar  | 200    | 是     |                                                              |          |
| ad                      | 卖点               |                      | varchar  | 200    | 是     |                                                              |          |
| type_picture            | 图片来源           |                      | int      | 2      | 是     | 类型1 产品库4 理货商推荐5 主动引用的信息6 商家自建           |          |
| picture_url             | 商品主图           |                      | varchar  | 200    | 是     |                                                              |          |
| type_price              | 定价来源           |                      | int      | 2      | 是     | 类型1 产品库2 总部推荐3 中心推荐4 理货商推荐5 主动引用的信息 |          |
| tv_sales_price          | 智慧云店销售价     |                      | decimal  | (14,2) | 是     | 原tv_price                                                   |          |
| tv_crossed_price        | 智慧云店划线价格   |                      | decimal  | (14,2) | 是     | 原crossed_price                                              |          |
| wei_sales_price         | 微商城销售价       |                      | decimal  | (14,2) | 是     | 原wei_price                                                  |          |
| wei_crossed_price       | 微商城划线价格     |                      | decimal  | (14,2) | 是     | 原crossed_price                                              |          |
| item_update_time        | 商品的更新时间     |                      | datetime |        |        | （价格变化才变）                                             |          |
| type_customize          |                    |                      | int      | 2      | 是     | 类型5 主动引用的信息6 商家自建                               |          |
| sales_terminal          | 虚拟销量           |                      | int      | 11     | 是     |                                                              |          |
| brokerage_price         | 佣金金额           |                      | decimal  | (14,2) | 是     |                                                              |          |
| promtion_rate           | 分销佣金比率       |                      | decimal  | (3,2)  | 是     |                                                              |          |
| item_label_ids          | 商品标签id         |                      | varchar  | 2000   | 是     | 用‘，’隔开，前面后面都有",",例如 ,1,2,',                     |          |
| item_label_names        | 商品标签名称       |                      | varchar  | 4000   | 是     | 用‘，’隔开，前面后面都有"","",例如 ,1,2,',                   |          |
| item_classify_ids       | 商品的易理货分类id |                      | varchar  | 2000   | 是     | 用‘，’隔开，前面后面都有"","",例如 ,1,2,',                   |          |
| item_classify_names     | 商品的易理货分类   |                      | varchar  | 4000   | 是     | 用‘，’隔开，前面后面都有"","",例如 ,1,2,',                   |          |
| industry_control_switch | 授权控制开关       |                      | int      | 2      | 是     | 1 开启  其他 不管                                            |          |
| cy_code                 | 产业code           |                      | varchar  | 20     | 是     |                                                              |          |
| gm_code                 | 中心code           |                      | varchar  | 10     | 是     |                                                              |          |
| create_time             | 创建日期           |                      | datetime |        |        |                                                              |          |
| update_time             | 修改日期           |                      | datetime |        |        |                                                              |          |
| create_user_id          | 创建人             |                      | int      | 11     | 是     |                                                              |          |
| update_user_id          | 修改人             |                      | int      | 11     | 是     |                                                              |          |
| version                 |                    |                      | int      | 11     | 是     |                                                              |          |

select * from target_classify; #目标分类
select * from actual_sales_details where order_id= 655 and `type` =2;//实际销售表
select * from target_classify_setting;
select * from target_month_sales ;
select * from target_year_sales;
select * from target_log;
select * from target_month_model_sales;//目标型号月追踪表
select * from target_month_main_push_goods_result;
select * from target_month_model_sales_item;

 select * from member_cy;  #产业表

 select * from member_gm;  #中心

```
#产品组名称查询
select 
 a.product_group_name 
 from 
 item_plat_product_group a
 left join item i on a.product_group_code = i.product_group_code
 left join item_sku s on i.id = s.id
 where 
  a.industry_code='ABA' 
  and a.industry_name='洗衣机'
  and s.id = '2000178';
```



#### 七、账号以及相关地址

git/svn账户

薛乐乐(lele.xue@dhc.com.cn)，登录用户名为：xuelele，登录密码为：S@7P8i$vVUJgNP$hOXIC

git库：http://12.168.3.18:9001/#/q/status:open

巨商汇开发平台：http://12.168.3.17/

生产网址：http://yilihuo.com/

预生产网址：http://pre.yilihuo.com/

测试网址：https://dev.yilihuo.com/ 

配host：

#巨商汇host配置
12.168.3.15     3bc1fda3.ngrok.io
12.168.3.15     dev.yilihuo.com

监控平台日志：

生产环境：http://monitor.yilihuo.com/login/index;JSESSIONID=ylh_session_prod_99ce4fcf7a2e43729a44d24d9885379e

测试环境：

http://12.168.3.17:8095/login/index;JSESSIONID=shiro_session_monitor_8df033fa999b461a9627d1330dc1d249

预生产环境：

http://premonitor.yilihuo.com/login/index;JSESSIONID=ylh_session_pre_22572eb3f3bd441fb20d4327876179b0

测试账号：

ylhtest1/123qwe ----买家
ylhtest/123qwe -----卖家

手机app

<http://dev.yilihuo.com/v2/app/>

<http://pre-m.yilihuo.com/#/>



DEV环境数据库账号密码 ：

注：该表格为dev环境数据库账号， 其中ylh_admin账号拥有数据修改权限，但不分配表结构修改权限；ylh_工程名 则为相应工程的数据库账号，用于本地连接dev库调试。

| 数据库账号      | 数据库密码       |
| --------------- | ---------------- |
| ylh_admin       | ylh_admin331     |
| ylh_cloud_goods | jshylhcloudgoods |
| ylh_base        | jshylhbase       |
| ylh_campaign    | jshylhcampaign   |
| ylh_ecm         | jshylhecm        |
| ylh_fund        | jshylhfund       |
| ylh_goods       | jshylhgoods      |
| ylh_notice      | jshylhnotice     |
| ylh_openapi     | jshylhopenapi    |
| ylh_order       | jshylhorder      |
| ylh_price       | jshylhprice      |
| ylh_user        | ylhuser991ui     |
| ylh_unipay      | jshylhunipay     |
| ylh_interface   | jshylhinterface  |

 