# 数据模式
## 第五章 结构类模式：抽象实体模式
 
程序中有抽象类。数据实体，本质上也是面向对象的，也都是人工智能领域里的实体框架结构相通。程序中面向对象以及设计模式均试图将抽象与具体分开，从而方便扩展。
   
数据也一样。实际业务中，我们可以通过定义抽象实体来解决复杂问题。
   
比如，一个新型家装电商平台项目，线上平台的参与方有平台方，装修公司，设计师，工长，材料供应商，监理以及装修客户。并且，可能一个人会分属以上多个类型，一个企业也有可能分属不同的类型。
   
在这个项目中，我们采用了抽像实体的模式，建立了参与方这个抽象实体表，同时，配上企业信息表与个人信息表。从而实现了所有参与方均用同一个注册登录接口。丢掉了用户表，部门表，员工表，供应商表，工长表，设计师表，装修公司表，监理表。从而大大减少了程序开发工作量。并且任一家企业都可以在此系统中管理自己企业的部门与员工，任何人，任何企业在这个平台，既可做甲方，也可以做乙方，视实际业务而定。开发上，使用抽象实体模式，4张表，1套代码。不用这个模式，则是8张表，8套代码。
    
业务伙伴表的实例如下:
```
-- ----------------------------
-- Table structure for xx_partner
-- ----------------------------
DROP TABLE IF EXISTS `xx_partner`;
CREATE TABLE `xx_partner`  (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '业务伙伴ID',
  `partner_name` varchar(60) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT '' COMMENT '业务伙伴名称（组织全称，个人姓名）',
  `partner_no` varchar(32) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT '' COMMENT '业务伙伴编号(员工，则是员工号）',
  `parent_id` int(11) NULL DEFAULT 0 COMMENT '上级部门ID',
  `level` tinyint(4) NULL DEFAULT 0 COMMENT '层级',
  `path` varchar(255) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT '' COMMENT '组织机构路径-ID路径串（逗号分隔）',
  `partner_type` int(11) NULL DEFAULT 0 COMMENT '业务伙伴类型（0:未定义，1，组织，2，个人）',
  `partner_role` int(11) NULL DEFAULT 0 COMMENT '业务伙伴角色 (平台，装修公司，设计师，工长，材料供应商，监理以及装修客户) ',
  `partner_grade` tinyint(1) NULL DEFAULT 0 COMMENT '业务伙伴分级（1、企业、2、部门（群体）、3、个人）',
  `has_child` tinyint(4) NULL DEFAULT 0 COMMENT '是否有子节点（校验部门是否有员工）',
  `is_user` tinyint(1) NULL DEFAULT 0 COMMENT '是否绑定过用户',
  `deleted` tinyint(1) NULL DEFAULT 0 COMMENT '是否删除',
  `create_by` int(11) NULL DEFAULT 0 COMMENT '录入人',
  `create_at` int(11) NULL DEFAULT 0 COMMENT '录入时间',
  `update_by` int(11) NULL DEFAULT 0 COMMENT '修改人',
  `update_at` int(11) NULL DEFAULT 0 COMMENT '修改时间',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `id`(`id`) USING BTREE,
  INDEX `parent_id`(`parent_id`) USING BTREE,
  INDEX `level`(`level`) USING BTREE,
  INDEX `partner_role`(`partner_role`) USING BTREE,
  INDEX `partner_type`(`partner_type`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 15 CHARACTER SET = utf8 COLLATE = utf8_unicode_ci COMMENT = '业务伙伴、往来单位' ROW_FORMAT = Compact;
```
   
实际上，当时，这一系统中有很多地方使用了这个模式。比如，菜单表就是这样。虽是一张表，但其中包括的是目录，菜单项，操作等
   
这一事例实际是树结构模式与抽象实体模式的结合应用。树结构的节点，是一个抽象实体。当我们赋与它类型，或角色，或者其它特征时，这就如同程序中实现的继承。
   
我们在开篇讲过一个实例，是一个包装管理的程序。本质上，那个程序，需要的是一个叫做“容器”的抽象实体。同时使用树结构模式，那么，就真实的展示了相应的包装层次的树型结构。同样也是一个树结构模式与抽像实体模式的结合应用。
    
采用了上述解决方案以后，面对不同客户的不同需求，我们只要根据客户产品的包装层次进行配置即可，程序根本不需要修改，即使修改，也不是这个需求层面的。

   

   
