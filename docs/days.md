## 上班第五周第二天

> 2022-01-11 

 迁移到notion

##  上班第五周第一天

> 2022-01-10

- new trace 修改
- trace 测试 cache 回滚
- 今天没接顺风车,准时上班
- 相册修改排序接口

## 上班第四周第四天

> 2022-01-07

- 文成婚礼延迟了

- 板桥月保续费了

- 新需求:消费的cmd进程 增加trace 

  ```
  span, ctx := tracer.SpanFromContext(context.Background())
    defer func() {
    span.Finish()
    }()
  ```

  		- Statistics done

  		- CmdTask done
  		- Income done 
  		- CmdScoreRefreshUser done 
  		- CmdScore done 
  		- CmdImMsgRecommend done
  		- HealthCheck done 
  		- CmdGuestRecommend done
  		- IntimacyCheck done 
  		- CmdMatch done

## 上班第三周第三天

> 2022-01-06

- 买个域名
- 域名实名认证
- App 跳转官网,增加生成官网token接口
- 公司年会,领个红包就可以走了啊,菜也不算好吃

## 上班第三周第二天

>2022-01-05 

- 修复消息自动推送 时间轮->延迟队列
- Nsq 延迟队列
- 推送fromUserId 是否看到

上班第三周第一天

> 2022-01-04 ,新年第一天上班,加油,虽然还是堵车了1个小时

- 学习
- 状态机
- 计算理论导引

## 上班第二周第五天

> 2021-12-30 

- 消息自动推送逻辑测试和bug修复

## 上班第二周第四天

> 2021-12-30 

- go-admin 改造
- 消息自动推送逻辑
- 加班到10点,

## 上班第二周第三天

> 2021-12-29 上班出地铁口以后,手机没电了,混进来公司才充电

- err warp add param 

- Go-admin 调研

  - 数据库必须要符合规范

    ```go
    type ModelTime struct {
        CreatedAt time.Time      `json:"createdAt" gorm:"comment:创建时间"`
        UpdatedAt time.Time      `json:"updatedAt" gorm:"comment:最后更新时间"`
        DeletedAt gorm.DeletedAt `json:"-" gorm:"index;comment:删除时间"`
    }
    type ControlBy struct {
        CreateBy int `json:"createBy" gorm:"index;comment:创建者"`
        UpdateBy int `json:"updateBy" gorm:"index;comment:更新者"`
    }
    ```

  - 生成代码以后必须要重启admin 
  - 查询显示的字段要自己编码(ui)
  - 需要用vue来编辑显示的字段
  - vue node 的依赖冲突有点坑

- layuiadmin 

  - Php 

    

## 上班第二周第二天

> 2021-12-28 上班堵车迟到了

- add trace  ctx 没有透传 trace 为 true
  - http 请求 
  - es请求
  - kafuka?

## 上班第二周第一天

> 2021-12-27

- 熟悉table store (aliyun)
- 熟悉php的动态接口
- 了解千万级别的feed流的设计
- aliyun tablestore go sdk learn
- 优化app/domain/comparepic -> library/aliyun hotfix/fix_comparepic

## 上班第五天 2021-12-24 平安夜

- 自测接口正常

- 自测返回结构

- 自测对比

  > 认证联系方式的时候,提示像素太低

```
ALTER TABLE `yougo`.`yougo_approve` 
MODIFY COLUMN `contact` varchar(512) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT '' COMMENT '联系方式认证' AFTER `app_id`;
ALTER TABLE `yougo`.`yougo_approve` CHARACTER SET = utf8mb4, COLLATE = utf8mb4_general_ci ;
```

```
INSERT INTO yougo_approve_contact_new SELECT uid,null,1 type ,fb contact ,fb_contact_status status ,fb_contact_reason reason ,dateline  from yougo_approve_contact  ;
INSERT INTO yougo_approve_contact_new SELECT uid,null,2 type ,line contact ,line_contact_status status ,line_contact_reason reason ,dateline  from yougo_approve_contact  ;
INSERT INTO yougo_approve_contact_new SELECT uid,null,3 type ,tw contact ,tw_contact_status status ,tw_contact_reason reason ,dateline  from yougo_approve_contact  ;
```

### 依赖接口而不是依赖实现





## 上班第四天 2021-12-23

- 上跳板机查看服务日志
- 个人资料检查,好长的一句if



### 联系方式验证优化

- 分支:feature/contact/auth -> feature/contact_auth_v2 
- 原表名: yougo_approve_contact 
- 新表名: yougo_approve_contact_new

```sql
CREATE TABLE `yougo_approve_contact_new` (
  `uid` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '用户id',
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '自增主键',
  `type` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '类型: 1 fb,2 line,3 tw,4 whatsapp',
  `contact` varchar(255) NOT NULL COMMENT '联系方式',
  `status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '认证状态 1 失败 2 成功',
  `reason` varchar(512) NOT NULL DEFAULT '' COMMENT '联系方式认证失败原因',
  `dateline` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '创建时间戳',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE KEY `uniq_uid_type` (`uid`,`type`) USING BTREE COMMENT '用户类型唯一索引',
  KEY `idx_uid_status` (`uid`,`status`) USING BTREE COMMENT '用户状态'
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COMMENT='yougo用户联系方式认证表v2'
```

- 前提

>  brew install protobuf
>
> go get github.com/favadi/protoc-go-inject-tag

- 生成modle 

  ```
  ./gen.sh yougo_approve_contact_new "mysql:root:@tcp(127.0.0.1:3306)/yougo"
  ```

  - ###### 涉及的接口

    ```
    /go/yougo/profile/contactauthen
    /go/yougo/auth/contact
    go/yougo/profile/getuserinfo
    ```

  		 - go/yougo/getuserinfo 接口修改完毕
  		 - /go/yougo/auth/contact 修改完毕
  		 - /go/yougo/profile/contactauthen 修改完毕

## 上班第三天 2021-12-22

- 使用自己的git账号(申请个账号权限用了两天)
- 设置跳板机,看下是否能使用
- 跳板机需要二部验证(mfa), sshpass不能交互,用iterm2的profiles和password

### 发现入职欢迎的图的我得职位变成了<<新转运营>>

### 默认文件修改

我们可以通过多种方式修改默认文件名称：

1. 通过配置管理方法`SetFileName`修改。
2. 修改命令行启动参数 - `gf.gcfg.file`。
3. 修改指定的环境变量 - `GF_GCFG_FILE`



### git缓存修改

- git stash save "说明"   //保存,跨branch
- git stash list  //列表
-  git stash pop //弹出并删除
- git stash apply/drop //应用不删除.删除

### 学习rpcx 

- hello world





### 上班第二天 2021-12-21

> 好久没见群姐(肠粉店老板娘)/9点10分上了地铁,10点05入办公室
>
> 番禺万达站换乘真是好远啊,换房住的话锁定 番禺广场OR 沙溪地铁站附件
>
> 今天好冷啊,出门穿了三件衣服才顶得住,回到办公室脱了外套

### goframe 学习日志1 

- 结构化约束,写死了提示信息,只能调试用吧,i18n可以处理 吗



### 旁边的人

> 一个写测试用例,兼职产品的运营小姐姐
>
> 项目只有一个产品,测试团队在武汉,有点蛋疼

### [Golang 新手可能会踩的 50 个坑](https://segmentfault.com/a/1190000013739000)

### consul 

- 服务发现
- 健康检查
- kv存储

### 聊天

- 异性聊天
- 付费聊天(只扣男的,平台和女号分钱)
- error code 和 多语言msg (有点丑)
- 判断有没性别设置/同性,可以优化代码
- SkipPay
- 在线状态
- channelName




