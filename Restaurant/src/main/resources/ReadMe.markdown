## 这个项目的学习get点
 * 扩展类DishDto
 * stream流的使用，比如给每个菜设置多个口味，这个时候往口味表添加这盘菜的数据时候：菜+口味。由于菜与口味是一对多的关系，因此往多表添加的时候，它使用了stream流的map方法，为每个口味设置菜品的id，添加到数据库中即可。
 * 插入完数据即可立马获取id
 * 批量删除的时候接收多个id组成的字符串，并以“，”隔开，此时可以直接使用List<Long>数组接收，但是要使用注解@RequestParam("id")
 * 写扩展类里的数组存储的泛型对象，要知道前端给你传的数组数据有哪些，应该用什么对象去接收，同时还要注意参数保持一致，否则接收不到（比如套餐与套餐里的菜品，添加一个套餐获取套餐id，扩展类Dto用套餐菜品关系类SetmealDish去接收前端传给的菜品数组setmealdishes，因为要操作的只有两张表：套餐表setmeal和套餐菜品关系表setmeal_dish，而扩展类的数组接收的菜品是要往关系表里添加的，而不是往菜品表里添加，要搞清楚关系）
 * MyBatisPlus对于时间的比较，条件构造器可以直接传入时间的字符串与对象的date属性进行比较，比如订单的page方法里的时间比较
 * 小程序端用户登录，输入手机号和收到的短信码，传入后端，后端可以直接使用Map集合接收！！！（普适性有待检测！！！！）
 * 配置类相当于之前的web项目中的web.xml
 
## 没搞清楚的点
 * 对于扩展类直接接收list集合的口味，是如何接收的，为什么使用一个DishDto就可以接收，前端到底以一种什么形式传的
 * 使用过滤器，在启动类上添加ServletComponentScan不起作用，反而在过滤器上添加注解Filter有用？
 
## 设计不合理的地方
 * 对于增删改，返回没有判断，几乎一致返回R.success("xxx成功！");应该加以判断
 * 前端设计添加购物车的时候不合理，一种菜只能一次性添加一次，如果需要丸子，一次只能添加一个；还有口味，一种只能选择一个...
 * 小程序端用session存储userId（可以不可以复用后端存储userId的方法，有待商贾） 
 * 购物车的删除，index.html页面只传了dishId和setmealId，那么如果出现相同dishId但是都是口味不同的菜，如果按照dishId删除，那么就删除了所有菜品
 
## 注意的点
 * BeanUtils.copyProperties不能一味的设置，比如套餐关系表与菜品，直接将菜品属性赋值给套擦菜品关系表导致的后果就是，不同的套餐里不能出现相同的菜，因为菜的id赋值给关系表当主键了
 * 向购物车添加菜品时候，要考虑出现（菜id和口味）一样的菜品时候修number即可，出现（菜名）一样但口味不同时候要添加，如果不存在添加即可（同时删除也要考虑这些因素）
 
## 逻辑稍稍难点
 * 购物车中菜品的添加以及套餐的添加
 * 扩展类的添加
 * 查看订单的时候封装OrderDto类型的Page（首先获取所有的订单，不对orders进行page，是对OrderDto进行page，然后对ordersList进行遍历并定义一个list集合存放OrderDto，然后遍历每个订单下所有detail，然后将每个订单复制并设置OrderDto，并设置商品件数，然后将每个OrderDto存放到list集合中，使用page，records设置未list集合即可）
 
