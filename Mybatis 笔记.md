---
title: Mybatis 笔记 
tags: 数据持久层
grammar_cjkRuby: true
---

### mybatis mapper 配置文件
```
<!-- mapper 根标签 -->
<mapper namespace="com.fintecher.manage.mapper.OrderProcessRecordMapper"></mapper>

<!--   获取还款详情列表 一对一 一对多  -->
<resultMap id="paymentScheduleDetailModel" autoMapping="true"
           type="com.fintecher.manage.vo.PaymentScheduleDetailModel">
    <id property="id" column="id"/>
    <association property="paymentTotalCount" autoMapping="true"
                 column="{orderId=id}" select="getPaymentTotalCount"/>
    <collection property="paymentDetails" autoMapping="true"
                column="{orderId=id}" select="getPaymentDetails"/>
</resultMap>

<select id="getPaymentScheduleDetail" resultMap="paymentScheduleDetailModel">
    SELECT
        a.id,
        b.name as customerName,
        a.order_number as orderNumber,
        c.client_number as clientNumber
    from product_order a
       left join personal b on  a.personal_id = b.id
       left join personal_bank c on b.id = c.personal_id
    where a.id = #{orderId}
</select>

<select id="getPaymentDetails" resultType="com.fintecher.manage.entity.PaymentSchedule">
    SELECT
        a.id,
        a.periods,
        a.principal_surplus as principalSurplus,
        a.interest_surplus as interestSuplus,
        a.penalty_surplus as penaltySurplus
    FROM payment_schedule a
    WHERE a.order_id = #{orderId}
</select>

<select id="getPaymentTotalCount" resultType="com.fintecher.manage.entity.PaymentSchedule">
    SELECT
        sum(IFNULL(a.sum,0)),
        sum(IFNULL(a.penal_sum,0)) as penalSum,
        sum(IFNULL(a.interest_surplus,0)) as interestSuplus,
        sum(IFNULL(a.penalty_surplus,0)) as penaltySurplus
    FROM payment_schedule a
    WHERE a.order_id = #{orderId}
    GROUP BY a.order_id
</select>
    
<!-- 参数类型 -->    
<select id="getAuditRecord" parameterType="com.fintecher.manage.vo.ApproveHistoryParam"
resultType="com.fintecher.manage.vo.ApproveHistoryModel">
    SELECT
    o.id AS orderId,
    ....
    ORDER BY h.operate_time DESC
</select>
```

### mybatis mapper java
```
@Repository
public interface GoodsOrderMapper extends MyMapper<GoodsOrder> {

    @Select("select id,order_status,order_money,created_time from goods_order where user_id = #{user_id}")
    public List<GoodsOrder> getGoodsOrderState(@Param("user_id") long user_id);
}
```