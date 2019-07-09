# springboot-spike
秒杀项目

pom文件
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

	<!--springboot最终打的jar包-->
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.wz</groupId>
	<artifactId>springboot-spike</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springboot-spike</name>
	<description>Spring Boot</description>

	<!--springboot父jar包-->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<!--jar版本-->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<!--springmvc-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!--lombok-->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.18.2</version>
			<optional>true</optional>
		</dependency>

		<!--redisson-->
		<dependency>
			<groupId>org.redisson</groupId>
			<artifactId>redisson</artifactId>
			<version>3.5.0</version>
		</dependency>

		<!--redis-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
		</dependency>

		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-pool2</artifactId>
			<version>2.0</version>
		</dependency>

		<dependency>
			<groupId>com.google.guava</groupId>
			<artifactId>guava</artifactId>
			<version>27.0.1-jre</version>
		</dependency>

		<!--加入springboot和mybatis整合包-->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.0</version>
		</dependency>

		<!--加入mysql驱动-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.0.5</version>
		</dependency>

		<!--加入阿里连接池-->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.29</version>
		</dependency>

		<!--加入阿里fastjson-->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.54</version>
		</dependency>

		<!--加入amqp-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-amqp</artifactId>
		</dependency>

		<!-- HSSF需要引入的 -->
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>RELEASE</version>
		</dependency>

		<!--Excel导出-->
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>3.15</version>
		</dependency>

		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml</artifactId>
			<version>3.15</version>
		</dependency>
	</dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!---pom配置一下编译xml文件-->
    <reports>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </reports>

</project>

application.yml
配置文件
server:
  port: 8080
redisson:
  address: redis://127.0.0.1:6379
spring:
  redis:
    lettuce:
      pool:
        max-active: 8 #最大活跃
        max-wait: -1ms #最大等待
        max-idle: 500 #最大空闲
        min-idle: 0 #最小空闲
    database: 0
    host: localhost
    port: 6379
    jedis:
      pool:
        max-active: 3000
        max-idle: 8
        min-idle: 0
        max-wait: 2000ms
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
    listener:
      direct:
          acknowledge-mode: MANUAL
      simple:
          acknowledge-mode: MANUAL

#设置数据库参数
  datasource:
    driver-class-name: com.mysql.jdbc.Driver #驱动
    url: jdbc:mysql://localhost:3306/seckill?useSSL=false&serverTimezone=UTC #连接
    username: root #用户名
    password: root #密码
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml #配置mybatis-config.xml文件，在文件中科院配置打印sql或mybatis相关配置
  mapper-locations: classpath:mybatis/mapper/*.xml #配置sql.xml文件的位置
  #mapper-locations: classpath:com/yunu/springboot/mapper/*.xml
  type-aliases-package: com.wz.springboot.model #配置实体路径

Commodity.xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.wz.springboot.dao.CommodityMapper" >
    <!--<resultMap id="BaseResultMap" type="com.yunu.springboot.model.Dp" >-->
        <!--<id column="id" property="id"/>-->
        <!--<result column="dpmc" property="dpmc"/>-->
        <!--<collection property="sp" ofType="com.yunu.springboot.model.Sp">-->
            <!--<id column="spid" property="spid"/>-->
            <!--<result column="spmc" property="spmc"/>-->
        <!--</collection>-->
    <!--</resultMap>-->

    <!--获取商品信息-->
    <select id="getSpxx" parameterType="com.wz.springboot.model.Commodity" resultType="com.wz.springboot.model.Commodity" >
      select t.id,t.spkc from commodity t where t.id = #{id}
    </select>

</mapper>

Limitedtimespike.xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.wz.springboot.dao.LimitedtimespikeMapper" >
    <!--<resultMap id="BaseResultMap" type="com.yunu.springboot.model.Dp" >-->
        <!--<id column="id" property="id"/>-->
        <!--<result column="dpmc" property="dpmc"/>-->
        <!--<collection property="sp" ofType="com.yunu.springboot.model.Sp">-->
            <!--<id column="spid" property="spid"/>-->
            <!--<result column="spmc" property="spmc"/>-->
        <!--</collection>-->
    <!--</resultMap>-->
    <!--获取限时秒杀商品-->
    <select id="getXsmssp" parameterType="com.wz.springboot.model.Limitedtimespike" resultType="com.wz.springboot.model.Limitedtimespike" >
        SELECT
            t.spid,
            t.spmsjg,
            t.mssj,
            t.msfz,
            a.spkc
        FROM
            limitedtimespike t,
            commodity a
        WHERE
            t.spid = a.id
            AND DATE_FORMAT( t.mssj, '%Y-%m-%d %H' ) = DATE_FORMAT( CURRENT_TIMESTAMP, '%Y-%m-%d %H' )
            AND t.lx = '0'
    </select>

</mapper>

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.wz.springboot.dao.PoiMapper" >
    <!--<resultMap id="BaseResultMap" type="com.yunu.springboot.model.Dp" >-->
        <!--<id column="id" property="id"/>-->
        <!--<result column="dpmc" property="dpmc"/>-->
        <!--<collection property="sp" ofType="com.yunu.springboot.model.Sp">-->
            <!--<id column="spid" property="spid"/>-->
            <!--<result column="spmc" property="spmc"/>-->
        <!--</collection>-->
    <!--</resultMap>-->

    <!--获取用户信息-->
    <select id="getYhxx" parameterType="java.lang.String" resultType="java.util.Map" >
        select t.xm,t.nl from teacher t where t.lx = #{lx} order by t.nl asc
    </select>

</mapper>


mybatis-config.xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--设置打印sql-->
        <setting name="logImpl" value="STDOUT_LOGGING" />
    </settings>
</configuration>

package com.wz.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.concurrent.ThreadPoolTaskScheduler;

@SpringBootApplication//springboot主启动类
@EnableScheduling
public class SpringbootApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootApplication.class, args);
	}

	@Bean
	public ThreadPoolTaskScheduler threadPoolTaskScheduler(){
		return new ThreadPoolTaskScheduler();
	}

}

package com.wz.springboot.service;

import com.alibaba.fastjson.JSONObject;
import com.wz.springboot.dao.CommodityMapper;
import com.wz.springboot.dao.LimitedtimespikeMapper;
import com.wz.springboot.model.Commodity;
import com.wz.springboot.model.CommodityDetails;
import com.wz.springboot.model.Limitedtimespike;
import com.wz.springboot.util.Constant;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.scheduling.concurrent.ThreadPoolTaskScheduler;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.concurrent.TimeUnit;
@Slf4j
@Service
public class SpikeServiceImpl implements SpikeService {

    @Autowired
    private AccessLimitService accessLimitService;
    @Autowired
    private RedisTemplate redisTemplate;
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    @Autowired
    private DistributedLocker distributedLocker;
    @Autowired
    private CommodityMapper commodityMapper;
    @Autowired
    private ThreadPoolTaskScheduler threadPoolTaskScheduler;
    @Autowired
    private LimitedtimespikeMapper limitedtimespikeMapper;

    //抢购秒杀
    @Override
    public String spike(Commodity com) throws Exception {
        //判断是否拿到令牌，是则继续，否则直接返回
        if(!accessLimitService.tryAcquireSeckill()){
            return rest("1000","抢购高峰，稍后重试");
        }
        //判断这个商品是否秒杀结束，是则返回，否则继续
        Integer spike = (Integer) redisTemplate.opsForValue().get(Constant.SPIKE+com.getId());
        if((spike == null ? 0 : spike) == 0){//0结束1未结束-商品id是否结束表示
            //秒杀结束主动删除缓存防止缓存中存在无用数据
//            String[] keys = {Constant.SPIKE+com.getId(),Constant.INCREASE+com.getId(),Constant.COMMODITY+com.getId()};
//            del(keys);
            return rest("1000","秒杀结束");
        }
        Integer increase = (Integer) redisTemplate.opsForValue().get(Constant.INCREASE+com.getId());
        Integer commodity = (Integer) redisTemplate.opsForValue().get(Constant.COMMODITY+com.getId());
        //判断这个商品是否秒杀自增>=库存，是则返回，否则继续
        if( increase >= commodity){//自增>=库存结束自增<库存未结束-自增id-商品id
            //商品已经售馨主动删除缓存防止缓存中存在无用数据
//            String[] keys = {Constant.SPIKE+com.getId(),Constant.INCREASE+com.getId(),Constant.COMMODITY+com.getId()};
//            del(keys);
            return rest("1000","商品已经售馨");
        }
        //判断这个请求是否重复，是则返回，否则继续
        CommodityDetails commodityDetails = (CommodityDetails)redisTemplate.opsForHash().get(Constant.ORDER+com.getId(),com.getYhid()+com.getId());//"唯一标识外key","用户id+商品id组合key"
        if(commodityDetails != null){
            return rest("1000","秒杀活动不能重复购买");
        }
        //真正下单使用mq或者redis锁都可以，串行化处理
        // 尝试获取锁，等待5秒，自己获得锁后一直不解锁则10秒后自动解锁
        boolean isGetLock = distributedLocker.tryLock(Constant.LOCK+com.getId(), TimeUnit.SECONDS, 5, 10);
        if (isGetLock) {
            //获取到锁做库存减1，并且生成redis订单
            long increment = redisTemplate.opsForValue().increment(Constant.INCREASE+com.getId(),1);
            //自增数已经自增到>库存证明商品已经卖完
            if(increment > commodity){//自增>库存结束自增<库存未结束
                // 释放锁
                //商品已经售馨主动删除缓存防止缓存中存在无用数据
//                String[] keys = {Constant.SPIKE+com.getId(),Constant.INCREASE+com.getId(),Constant.COMMODITY+com.getId()};
//                del(keys);
                distributedLocker.unlock(Constant.LOCK+com.getId());
                return rest("1000","商品秒杀结束");
            }
            //自增数<=库存证明商品可以卖
            CommodityDetails commoditydetails = new CommodityDetails();
            commoditydetails.setYhId(com.getYhid());//用户id
            commoditydetails.setSpId(com.getId());//商品id
            commoditydetails.setSpJg(com.getSpmsjg());//商品价格
            //插入redis缓存订单,并且设置10分钟不付款自动取消订单，并且设置redis删除key触发事件在事件内打印key，其实自动收货可以借助这种方式，key过期触发事件中写自动收货的逻辑
            redisTemplate.opsForHash().put(Constant.ORDER+com.getId(),com.getYhid()+com.getId(),commoditydetails);
            redisTemplate.expire(Constant.ORDER+com.getId(),10,TimeUnit.SECONDS);
            //模拟调用微信支付
            log.info("模拟调用微信支付");
            Thread.sleep(10);
            // 释放锁
            distributedLocker.unlock(Constant.LOCK+com.getId());
            return rest("1001","生成订单成功");
        }
        return null;
    }

    //微信回调
    @Override
    public String weixinSpike() throws Exception {
        //判断微信返回的状态，成功则成功，失败则失败，失败处理自增数减1并且删除redis订单
        if(!false){//支付失败
            //库存减1，并且redis删除订单
            redisTemplate.opsForValue().set("f",(Integer)redisTemplate.opsForValue().get("f") - 1);
            redisTemplate.opsForHash().delete("c","d");
            return "抢购失败";
        }
        //支付成功
        //模拟插入数据库订单
        Thread.sleep(10);
        return "抢购成功";
    }

    //自动开启秒杀
    @Override
//    @Scheduled(cron = "0 */1 * * * ?")
//    @Scheduled(cron = "*/1 * * * * ?")
    public void cronlimitedtimeSpike() throws Exception {
        //查询商品表是否有商品秒杀，有则开始，无则不开始
        Limitedtimespike limitedtimespike = limitedtimespikeMapper.getXsmssp();
        if(limitedtimespike == null){
            log.info("限时秒杀未开始");
        }else{
            Integer spike = (Integer) redisTemplate.opsForValue().get(Constant.SPIKE+limitedtimespike.getSpid());
            if((spike == null ? 0 : spike) == 0 ){
                log.info("限时秒杀开始");
                //初始化库存信息和自增数和商品秒杀标识
                redisTemplate.opsForValue().set(Constant.SPIKE+limitedtimespike.getSpid(),1,limitedtimespike.getMsfz(),TimeUnit.MINUTES);
                redisTemplate.opsForValue().set(Constant.COMMODITY+limitedtimespike.getSpid(),limitedtimespike.getSpkc(),limitedtimespike.getMsfz(),TimeUnit.MINUTES);
                redisTemplate.opsForValue().set(Constant.INCREASE+limitedtimespike.getSpid(),0,limitedtimespike.getMsfz(),TimeUnit.MINUTES);
            }else{
                log.info("限时秒杀正在进行中");
            }
        }
    }

    //手动开启秒杀
    @Override
    public String limitedtimeSpike(Commodity commodity) {
        //查询商品表是否有商品秒杀，有则开始，无则不开始
        Commodity comm = commodityMapper.getSpxx(commodity);
        if(commodity.getSpkc()>comm.getSpkc()){
            return rest("1000","秒杀价格大于商品价格");
        }
        Integer spike = (Integer) redisTemplate.opsForValue().get(Constant.SPIKE+comm.getId());
        if((spike == null ? 0 : spike) > 0 ){
            return rest("1000","商品正在秒杀中");
        }
        System.out.println(redisTemplate);
        //手动秒杀，redis的key失效时间取决于秒杀多长时间
        redisTemplate.opsForValue().set(Constant.SPIKE+comm.getId(),1,commodity.getMssc(),TimeUnit.MINUTES);//秒杀标识
        redisTemplate.opsForValue().set(Constant.COMMODITY+comm.getId(),commodity.getSpkc(),commodity.getMssc(),TimeUnit.MINUTES);//商品
        redisTemplate.opsForValue().set(Constant.INCREASE+comm.getId(),0,commodity.getMssc(),TimeUnit.MINUTES);//自增
        return rest("1001","手动秒杀开始");
    }

    //统一返回格式封装
    protected String rest(String code,String msg){
        JSONObject jsonobject = new JSONObject();
        jsonobject.put("code",code);
        jsonobject.put("msg",msg);
        return jsonobject.toJSONString();
    }

    //秒杀出现结束或者售罄情况删除缓存
    protected void del(String[] keys){
        redisTemplate.delete(new ArrayList<>(Arrays.asList(keys)));
    }
}

package com.wz.springboot.service;

import com.wz.springboot.model.Commodity;

public interface SpikeService {

    public String spike(Commodity commodity) throws Exception;

    public String weixinSpike() throws Exception;

    public void cronlimitedtimeSpike() throws Exception;

    public String limitedtimeSpike(Commodity commodity);

}

package com.wz.springboot.service;

import org.redisson.api.RLock;
import org.redisson.api.RedissonClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.concurrent.TimeUnit;

@Service
public class RedissonDistributedLocker implements DistributedLocker{

    // RedissonClient已经由配置类生成，这里自动装配即可
    @Autowired
    private RedissonClient redissonClient;

    // lock(), 拿不到lock就不罢休，不然线程就一直block
    @Override
    public RLock lock(String lockKey) {
        RLock lock = redissonClient.getLock(lockKey);
        lock.lock();
        return lock;
    }

    // leaseTime为加锁时间，单位为秒
    @Override
    public RLock lock(String lockKey, long leaseTime) {
        RLock lock = redissonClient.getLock(lockKey);
        lock.lock(leaseTime, TimeUnit.SECONDS);
        return null;
    }

    // timeout为加锁时间，时间单位由unit确定
    @Override
    public RLock lock(String lockKey, TimeUnit unit, long timeout) {
        RLock lock = redissonClient.getLock(lockKey);
        lock.lock(timeout, unit);
        return lock;
    }

    // 尝试获取锁，等待waitTime秒，自己获得锁后一直不解锁则leaseTime秒后自动解锁，unit单位
    @Override
    public boolean tryLock(String lockKey, TimeUnit unit, long waitTime, long leaseTime) {
        RLock lock = redissonClient.getLock(lockKey);
        try {
            return lock.tryLock(waitTime, leaseTime, unit);
        } catch (InterruptedException e) {
            return false;
        }
    }

    //释放锁
    @Override
    public void unlock(String lockKey) {
        RLock lock = redissonClient.getLock(lockKey);
        lock.unlock();
    }

    //指定RLock对象释放锁
    @Override
    public void unlock(RLock lock) {
        lock.unlock();
    }

}

package com.wz.springboot.service;

import com.wz.springboot.configure.RabbitConfig;
import org.springframework.amqp.core.MessageDeliveryMode;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;

import static com.sun.tools.doclint.Entity.trade;

public class Rabbitmq {
//    @Autowired
//    RabbitTemplate rabbitTemplate;
//    public void convertAndSend(){
//    // 通过广播模式发布延时消息 延时30分钟 持久化消息 消费后销毁 这里无需指定路由，会广播至每个绑定此交换机的队列
//    rabbitTemplate.convertAndSend(RabbitConfig.Delay_Exchange_Name, "", trade.getTradeCode(), message ->{
//        message.getMessageProperties().setDeliveryMode(MessageDeliveryMode.PERSISTENT);
//        message.getMessageProperties().setDelay(30 * (60*1000));   // 毫秒为单位，指定此消息的延时时长
//        return message;
//    });
//    }
}


package com.wz.springboot.service;

import org.apache.poi.hssf.usermodel.HSSFWorkbook;

import java.io.IOException;

public interface PoiServiceImpl {

    public HSSFWorkbook excel(String lx) throws IOException;

}


package com.wz.springboot.service;

import com.wz.springboot.dao.PoiMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.*;

@Service
public class PoiService implements PoiServiceImpl {

    @Autowired
    private PoiMapper poiMapper;

    @Override
    public List<Map<String,String>> excel(String lx) {
        List<Map<String,String>> list = poiMapper.getYhxx(lx);
        return list;
    }
}



package com.wz.springboot.service;

import com.wz.springboot.configure.RabbitConfig;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.amqp.core.Message;
import com.rabbitmq.client.Channel;

import java.io.IOException;

@Component
public class PayTimeOutConsumer {

//    @Autowired
//    TradeService tradeService;
//
//    private Logger logger = LoggerFactory.getLogger(getClass());
//
//    @RabbitListener(queues = RabbitConfig.Timeout_Trade_Queue_Name)
//    public void process(String tradeCode, Message message, Channel channel) throws IOException {
//        try {
//            logger.info("开始执行订单[{}]的支付超时订单关闭......", tradeCode);
//            tradeService.cancelTrade(tradeCode);
//            channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
//            logger.info("超时订单处理完毕");
//        } catch (Exception e) {
//            logger.error("超时订单处理失败:{}", ExceptionUtil.getMessage(e));
//            channel.basicReject(message.getMessageProperties().getDeliveryTag(), false);
//        }
//    }
}


package com.wz.springboot.service;

import org.redisson.api.RLock;

import java.util.concurrent.TimeUnit;

public interface DistributedLocker {

    public RLock lock(String lockKey);

    public RLock lock(String lockKey, long timeout);

    public RLock lock(String lockKey, TimeUnit unit, long timeout);

    public boolean tryLock(String lockKey, TimeUnit unit, long waitTime, long leaseTime);

    public void unlock(String lockKey);

    public void unlock(RLock lock);
}


package com.wz.springboot.service;

import com.google.common.util.concurrent.RateLimiter;
import org.springframework.stereotype.Service;

@Service
public class AccessLimitServiceImpl implements  AccessLimitService{
    //限流工具设置10个令牌，拿到令牌才可以秒杀抢购
    private RateLimiter rateLimiter = RateLimiter.create(10);
    //获取令牌
    @Override
    public boolean tryAcquireSeckill() {
        return rateLimiter.tryAcquire();
    }
}

package com.wz.springboot.service;

public interface AccessLimitService {

    public boolean tryAcquireSeckill();
}

package com.wz.springboot.model;

import lombok.Data;

@Data
public class Teacher {

    private String xm;//姓名

    private String nl;//年龄


}


package com.wz.springboot.model;

import lombok.Data;

import java.math.BigDecimal;

//限时秒杀实体
@Data
public class Limitedtimespike {

    private String spid;//商品id

    private BigDecimal spmsjg;//商品秒杀价格（单位元保留两位小数）

    private String mssj;//秒杀时间

    private Integer msfz;//秒杀时长（单位分钟）

    private Integer spkc;//库存（单位个）
}


package com.wz.springboot.model;

import lombok.Data;

import java.io.Serializable;
import java.math.BigDecimal;

@Data
public class CommodityDetails implements Serializable{

    private String yhId;//用户id
    private String spId;//商品id
    private BigDecimal spJg;//商品价格
}


package com.wz.springboot.model;

import lombok.Data;

import java.math.BigDecimal;

//商品实体
@Data
public class Commodity {

    private String id;//商品id

    private Integer spkc;//商品库存

    private Integer mssc;//秒杀时长（单位分钟）

    private BigDecimal spmsjg;//商品秒杀价格（元保留两位小数）

    private String yhid;//用户id
}


package com.wz.springboot.dao;

import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;
import java.util.Map;

@Mapper
@Repository
public interface PoiMapper {

    public List<Map<String,String>> getYhxx(String lx);
}


package com.wz.springboot.dao;

import com.wz.springboot.model.Commodity;
import com.wz.springboot.model.Limitedtimespike;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

//表示此类是映射类
@Mapper
@Repository
public interface LimitedtimespikeMapper {

    public Limitedtimespike getXsmssp();

}


package com.wz.springboot.dao;

import com.wz.springboot.model.Commodity;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

//表示此类是映射类
@Mapper
@Repository
public interface CommodityMapper {

    public Commodity getSpxx(Commodity commodity);

}


package com.wz.springboot.controller;

import com.wz.springboot.model.Commodity;
import com.wz.springboot.service.DistributedLocker;
import com.wz.springboot.service.SpikeService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.TimeUnit;

@RestController
@RequestMapping("/redisson")
public class SpikeController {
    @Autowired
    private DistributedLocker distributedLocker;
    @Autowired
    private SpikeService lockTestService;

    @RequestMapping("/test")
    public void redissonTest() {
        String key = "redisson_key";
        for (int i = 0; i < 100; i++) {
            Thread t = new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        /*
                         * 直接加锁，获取不到锁则一直等待获取锁
                         * distributedLocker.lock(key,10L);
                         * 获得锁之后可以进行相应的处理
                         * Thread.sleep(100);
                         * System.err.println("======获得锁后进行相应的操作======"+Thread.currentThread().getName());
                         * 解锁
                         * distributedLocker.unlock(key);
                         */
                        // 尝试获取锁，等待5秒，自己获得锁后一直不解锁则10秒后自动解锁
                        boolean isGetLock = distributedLocker.tryLock(key, TimeUnit.SECONDS, 5L, 10L);
                        if (isGetLock) {
                            // 获得锁之后可以进行相应的处理
                            System.out.println("线程:" + Thread.currentThread().getName() + ",获取到了锁");
                            Thread.sleep(100);
                            System.err.println("======获得锁后进行相应的操作======" + Thread.currentThread().getName());
                            // 释放锁
                            distributedLocker.unlock(key);
                            System.err.println("================释放锁=============" + Thread.currentThread().getName());
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            });
            t.start();
        }
    }

    //抢购秒杀
    @RequestMapping("/spike")
    public String spike(@RequestBody Commodity commodity) throws Exception {
        return lockTestService.spike(commodity);
    }

    //微信回调
    @RequestMapping("/weixinSpike")
    public String weixinSpike() throws Exception {
        return lockTestService.weixinSpike();
    }

    //手动开启秒杀
    @PostMapping("/limitedtimeSpike")
    public String limitedtimeSpike(@RequestBody Commodity commodity) {
        return lockTestService.limitedtimeSpike(commodity);
    }
}
package com.wz.springboot.controller;

import com.wz.springboot.service.PoiService;
import com.wz.springboot.util.PoiUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import javax.servlet.http.HttpServletResponse;
import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/poi")
public class PoiController {

    @Autowired
    private PoiService poiService;

    //使用poi导出excel
    @GetMapping("/excel")
    public void excel(@RequestParam String lx, HttpServletResponse response) {
        List<Map<String,String>> list = poiService.excel(lx);
        PoiUtil.excel(list,response);
    }
}



package com.wz.springboot.configure;

import com.wz.springboot.util.Constant;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.connection.Message;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.listener.KeyExpirationEventMessageListener;
import org.springframework.data.redis.listener.RedisMessageListenerContainer;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.concurrent.TimeUnit;

//监听redis过期的key，如果过期则处理事件，例如10分钟不支付则默认是取消订单，15天默认自动收货
@Slf4j
public class KeyExpiredListener extends KeyExpirationEventMessageListener {

    @Autowired
    private RedisTemplate redisTemplate;

    public KeyExpiredListener(RedisMessageListenerContainer listenerContainer) {
        super(listenerContainer);
    }

    @Override
    public void onMessage(Message message, byte[] pattern) {
        String channel = new String(message.getChannel(), StandardCharsets.UTF_8);
        String key = new String(message.getBody(),StandardCharsets.UTF_8);
        log.info("redis key 过期：pattern={},channel={},key={}",new String(pattern),channel,key);
        if(key.startsWith(Constant.ORDER)){//订单10分钟未支付，默认未失效，则删除订单和库存+1
            String order = key.split("_")[1].toString();
            log.info("订单未支付，自动取消订单:"+order);
            String[] keys = {key};
            del(keys);
            key = Constant.INCREASE+order;
            Integer increase = (Integer) redisTemplate.opsForValue().get(key)-1;
            redisTemplate.opsForValue().set(key,increase,redisTemplate.getExpire(key),TimeUnit.SECONDS);//自增-1
        }else if(key.startsWith(Constant.AUTOMATICRECEIVING)){//15天自动收货
            log.info("订单自动收货:"+key);
        }else{
            log.info("key:"+key);
        }
    }

    //订单未支付删除缓存
    protected void del(String[] keys){
        redisTemplate.delete(new ArrayList<>(Arrays.asList(keys)));
    }
}


package com.wz.springboot.configure;

import java.util.HashMap;
import java.util.Map;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.FanoutExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.amqp.rabbit.connection.ConnectionFactory;
import org.springframework.amqp.rabbit.core.RabbitAdmin;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;



/**
 * @title rabbitmq配置类
 * @author zengzp
 * @time 2018年8月20日 上午10:46:43
 * @Description
 */
@Configuration
public class RabbitConfig {

    // 支付超时延时交换机
    public static final String Delay_Exchange_Name = "delay.exchange";

    // 超时订单关闭队列
    public static final String Timeout_Trade_Queue_Name = "close_trade";


    @Bean
    public Queue delayPayQueue() {
        return new Queue(RabbitConfig.Timeout_Trade_Queue_Name, true);
    }


    // 定义广播模式的延时交换机 无需绑定路由
    @Bean
    FanoutExchange delayExchange(){
        Map<String, Object> args = new HashMap<String, Object>();
        args.put("x-delayed-type", "direct");
        FanoutExchange topicExchange = new FanoutExchange(RabbitConfig.Delay_Exchange_Name, true, false, args);
        topicExchange.setDelayed(true);
        return topicExchange;
    }

    // 绑定延时队列与交换机
    @Bean
    public Binding delayPayBind() {
        return BindingBuilder.bind(delayPayQueue()).to(delayExchange());
    }

    // 定义消息转换器
    @Bean
    Jackson2JsonMessageConverter jsonMessageConverter() {
        return new Jackson2JsonMessageConverter();
    }

    // 定义消息模板用于发布消息，并且设置其消息转换器
    @Bean
    RabbitTemplate rabbitTemplate(final ConnectionFactory connectionFactory) {
        final RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
        rabbitTemplate.setMessageConverter(jsonMessageConverter());
        return rabbitTemplate;
    }
    @Bean
    RabbitAdmin rabbitAdmin(final ConnectionFactory connectionFactory) {
        return new RabbitAdmin(connectionFactory);
    }

}


package com.wz.springboot.configure;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.listener.ChannelTopic;
import org.springframework.data.redis.listener.RedisMessageListenerContainer;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
@EnableCaching
public class RedisConfig {

    // 以下两种redisTemplate自由根据场景选择
    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        Jackson2JsonRedisSerializer serializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper mapper = new ObjectMapper();
        mapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        mapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        serializer.setObjectMapper(mapper);
        template.setValueSerializer(serializer);
        //使用StringRedisSerializer来序列化和反序列化redis的key值
        template.setKeySerializer(new StringRedisSerializer());//这两句是关键
        template.setHashKeySerializer(new StringRedisSerializer());//这两句是关键
        template.afterPropertiesSet();
        return template;
    }
    @Bean
    public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory factory) {
        StringRedisTemplate stringRedisTemplate = new StringRedisTemplate();
        stringRedisTemplate.setConnectionFactory(factory);
        return stringRedisTemplate;
    }

    @Autowired
    private RedisConnectionFactory redisConnectionFactory;

    @Bean
    public RedisMessageListenerContainer redisMessageListenerContainer() {
        RedisMessageListenerContainer redisMessageListenerContainer = new RedisMessageListenerContainer();
        redisMessageListenerContainer.setConnectionFactory(redisConnectionFactory);
        return redisMessageListenerContainer;
    }

    @Bean
    public KeyExpiredListener keyExpiredListener() {
        return new KeyExpiredListener(this.redisMessageListenerContainer());
    }

}


package com.wz.springboot.configure;

import org.redisson.Redisson;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RedissonManager {

    @Value("${redisson.address}")
    private String addressUrl;

    @Bean
    public RedissonClient getRedisson() throws Exception {
        RedissonClient redisson = null;
        Config config = new Config();
        config.useSingleServer().setAddress(addressUrl);//指定ip和端口
        redisson = Redisson.create(config);//生成RedissonClient客户端
        System.out.println(redisson.getConfig().toJSON().toString());
        return redisson;
    }
}


package com.wz.springboot.util;

import lombok.extern.slf4j.Slf4j;
import org.apache.poi.hssf.usermodel.*;
import org.apache.poi.hssf.util.HSSFColor;
import org.apache.poi.ss.usermodel.FillPatternType;
import org.apache.poi.ss.usermodel.HorizontalAlignment;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.OutputStream;
import java.net.URLEncoder;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;
import java.util.Map;
import java.util.UUID;

@Slf4j
public class PoiUtil {

    public static HSSFWorkbook createExcel(List<Map<String,String>> list) {
        // 创建一个webbook，对应一个Excel文件
        HSSFWorkbook wb = new HSSFWorkbook();
        // 在webbook中添加一个sheet,对应Excel文件中的sheet
        HSSFSheet sheet = wb.createSheet("报表");
        // 默认列宽
        sheet.setDefaultColumnWidth(20);
        // 在sheet中添加表头第0行
        HSSFRow row = sheet.createRow(0);
        // 创建单元格，并设置值表头 设置表头居中 背景颜色 前景颜色
        HSSFCellStyle style = wb.createCellStyle();
        // 创建一个居中格式
        style.setAlignment(HorizontalAlignment.CENTER);
        //设置背景颜色
        style.setFillForegroundColor(HSSFColor.LIME.index);
        //前景颜色
        style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
        // 添加excel title
        String[] strArray = excelTitle();
        for (int i = 0; i < strArray.length; i++) {
            HSSFCell cell = row.createCell(i);
            cell.setCellValue(strArray[i]);
            cell.setCellStyle(style);
        }

        //写入数据
        for (int i = 0;i<list.size();i++) {
            row = sheet.createRow(i + 1);//创建行
            int j = 0;
            //创建单元格，并设置值
            for(Map.Entry entry : list.get(i).entrySet()){
                HSSFCell hssfCell = row.createCell(j);
                if(null!=entry.getValue()){
                    hssfCell.setCellValue((String) entry.getValue());
                }else{
                    hssfCell.setCellValue("");
                }
                j++;
            }
        }
        return wb;
    }

    public static String[] excelTitle() {
        String[] strArray = {"姓名", "年龄" };
        return strArray;
    }

    public static void excel(List<Map<String,String>> list,HttpServletResponse response){
        try {
            HSSFWorkbook hssfWorkbook = createExcel(list);
            OutputStream outputStream = response.getOutputStream();
            hssfWorkbook.write(response.getOutputStream());
            response.setContentType("application/vnd.ms-excel");
            response.setHeader("Content-Disposition", "attachment;filename="+ URLEncoder.encode(new SimpleDateFormat("yyyy-MM-dd").format(new Date())+"-"+ UUID.randomUUID().toString().replace("-","")+".xls", "utf-8"));
            outputStream.flush();
            outputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}



/*
 Navicat Premium Data Transfer

 Source Server         : test
 Source Server Type    : MySQL
 Source Server Version : 50519
 Source Host           : localhost:3306
 Source Schema         : seckill

 Target Server Type    : MySQL
 Target Server Version : 50519
 File Encoding         : 65001

 Date: 09/07/2019 16:14:12
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for commodity
-- ----------------------------
DROP TABLE IF EXISTS `commodity`;
CREATE TABLE `commodity`  (
  `id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主键',
  `spmc` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '商品名称',
  `spjg` decimal(10, 2) DEFAULT NULL COMMENT '商品价格',
  `spjs` text CHARACTER SET utf8 COLLATE utf8_general_ci COMMENT '商品介绍',
  `spkc` int(30) DEFAULT NULL COMMENT '商品库存',
  `sjsj` datetime DEFAULT NULL COMMENT '上架时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '商品表' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of commodity
-- ----------------------------
INSERT INTO `commodity` VALUES ('6bd3daf99ef011e9ac724ccc6afb77c9', '泳衣', 510.01, '泳装是指在水中或海滩活动时和模特及选美时展示形体的专用服装。有一件式和两截式和三点式（比基尼）等变化。', 10, '2019-07-05 14:45:52');

-- ----------------------------
-- Table structure for limitedtimespike
-- ----------------------------
DROP TABLE IF EXISTS `limitedtimespike`;
CREATE TABLE `limitedtimespike`  (
  `id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主键',
  `spid` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '商品id',
  `spmsjg` decimal(10, 2) DEFAULT NULL COMMENT '商品秒杀价格',
  `mssj` datetime DEFAULT NULL COMMENT '秒杀时间',
  `msfz` int(10) DEFAULT NULL COMMENT '秒杀分钟',
  `lx` varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '类型0未开始1已开始2已结束',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '限时秒杀表' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of limitedtimespike
-- ----------------------------
INSERT INTO `limitedtimespike` VALUES ('61adce669efe11e9ac724ccc6afb77c9', '6bd3daf99ef011e9ac724ccc6afb77c9', 10.00, '2019-07-05 17:00:00', 10, '0');

-- ----------------------------
-- Table structure for order
-- ----------------------------
DROP TABLE IF EXISTS `order`;
CREATE TABLE `order`  (
  `id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主键',
  `spid` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '商品id',
  `yhid` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '用户id',
  `gmsj` datetime DEFAULT NULL COMMENT '购买时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '订单表' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of order
-- ----------------------------
INSERT INTO `order` VALUES ('6bd3daf99ef011e9ac724ccc6afb77c9', '泳衣', NULL, '2019-07-05 14:45:52');

-- ----------------------------
-- Table structure for secondkillrecord
-- ----------------------------
DROP TABLE IF EXISTS `secondkillrecord`;
CREATE TABLE `secondkillrecord`  (
  `id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主键',
  `spid` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '商品id',
  `spmsjg` decimal(10, 2) DEFAULT NULL COMMENT '商品秒杀价格',
  `mssj` datetime DEFAULT NULL COMMENT '秒杀时间',
  `msfz` int(10) DEFAULT NULL COMMENT '秒杀分钟',
  `lx` varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '类型1已开始2已结束',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '秒杀记录表' ROW_FORMAT = Compact;

-- ----------------------------
-- Table structure for teacher
-- ----------------------------
DROP TABLE IF EXISTS `teacher`;
CREATE TABLE `teacher`  (
  `id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主键',
  `xm` varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '姓名',
  `nl` varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '年龄',
  `lx` varchar(1) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '类型',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '导出表' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of teacher
-- ----------------------------
INSERT INTO `teacher` VALUES ('01b3d8c2a15f11e9ac724ccc6afb77c9', '王满宝', '20', '1');
INSERT INTO `teacher` VALUES ('05f8d971a15f11e9ac724ccc6afb77c9', '王芬芬', '19', '1');
INSERT INTO `teacher` VALUES ('09b74c25a15f11e9ac724ccc6afb77c9', '王二芬', '18', '1');
INSERT INTO `teacher` VALUES ('0d29b84ca15f11e9ac724ccc6afb77c8', '王佳敏', '16', '1');
INSERT INTO `teacher` VALUES ('0d29b84ca15f11e9ac724ccc6afb77c9', '王嫂嫂', '17', '0');

-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user`  (
  `id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主键',
  `yhmc` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '用户名称',
  `yhmm` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '用户密码',
  `zcsj` datetime DEFAULT NULL COMMENT '注册时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '用户表' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('6bd3daf99ef011e9ac724ccc6afb77c9', 'wmb', '123456', '2019-07-05 14:45:52');

SET FOREIGN_KEY_CHECKS = 1;


notify-keyspace-events "Ex"

------------------------
https://github.com/battcn/spring-boot2-learning
https://github.com/vector4wang/spring-boot-quick/tree/master/quick-rabbitmq
https://github.com/vector4wang/spring-boot-quick


