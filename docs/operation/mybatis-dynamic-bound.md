参考： http://blog.csdn.net/small____fish/article/details/8029030/

foreach的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。foreach元素的属性主要有item，index，collection，open，separator，close。item表示集合中每一个元素进行迭代时的别名，index指定一个名字，用于表示在迭代过程中，每次迭代到的位置，open表示该语句以什么开始，separator表示在每次进行迭代之间以什么符号作为分隔符，close表示以什么结束，在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况下，该属性的值是不一样的，主要有一下3种情况：

1. 如果传入的是单参数且参数类型是一个List的时候，collection属性值为list .
2. 如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array .
3. 如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了，当然单参数也可以封装成map，实际上如果你在传入参数的时候，在MyBatis里面也是会把它封装成一个Map的，map的key就是参数名，所以这个时候collection属性值就是传入的List或array对象在自己封装的map里面的key.

下面我们通过代码实践:

数据表:

采用Oracle的HR.Employees表

​实体:Employees

public class Employees {
    private Integer employeeId;
    private String firstName;
    private String lastName;
    private String email;
    private String phoneNumber;
    private Date hireDate;
    private String jobId;
    private BigDecimal salary;
    private BigDecimal commissionPct;
    private Integer managerId;
    private Short departmentId;
}
映射文件:    **<!--List:forech中的collection属性类型是List,collection的值必须是:list,item的值可以随意,Dao接口中参数名字随意 -->    <select id="getEmployeesListParams" resultType="Employees">        select \*        from EMPLOYEES e        where e.EMPLOYEE_ID in        <foreach collection="list" item="employeeId" index="index"            open="(" close=")" separator=",">            #{employeeId}        </foreach>    </select>    <!--Array:forech中的collection属性类型是array,collection的值必须是:list,item的值可以随意,Dao接口中参数名字随意 -->    <select id="getEmployeesArrayParams" resultType="Employees">        select *        from EMPLOYEES e        where e.EMPLOYEE_ID in        <foreach collection="array" item="employeeId" index="index"            open="(" close=")" separator=",">            #{employeeId}        </foreach>    </select>    <!--Map:不单单forech中的collection属性是map.key,其它所有属性都是map.key,比如下面的departmentId -->    <select id="getEmployeesMapParams" resultType="Employees">        select *        from EMPLOYEES e        <where>            <if test="departmentId!=null and departmentId!=''">                e.DEPARTMENT_ID=#{departmentId}            </if>            <if test="employeeIdsArray!=null and employeeIdsArray.length!=0">                AND e.EMPLOYEE_ID in                <foreach collection="employeeIdsArray" item="employeeId"                    index="index" open="(" close=")" separator=",">                    #{employeeId}                </foreach>            </if>        </where>    </select>**Mapper类:
public interface EmployeesMapper {
    List<Employees> getEmployeesListParams(List<String> employeeIds);
    List<Employees> getEmployeesArrayParams(String[] employeeIds);
    List<Employees> getEmployeesMapParams(Map<String,Object> params);
}

测试方法(未贴完整代码):

​    @Test
    public void testGetEmployeesListParams() {
        List<String> employeeIds = Arrays.asList("100", "101", "200");
        List<Employees> result = employeesMapper
                .getEmployeesListParams(employeeIds);
        assertEquals(3, result.size());
    }
    @Test
    public void testGetEmployeesArrayParams() {
        String[] employeeIds = new String[] { "100", "200" };
        List<Employees> result = employeesMapper
                .getEmployeesArrayParams(employeeIds);
        assertEquals(2, result.size());
    }
    @Test
    public void testGetEmployeesMapParams() {
        String departmentId = "60";
        List<String> employeeIdsList = Arrays.asList("103", "104", "105");
        String[] employeeIdsArray = new String[] { "103", "104" };
        // 传入多个参数
        Map<String, Object> params = new HashMap<String, Object>();
        params.put("departmentId", departmentId);
        params.put("employeeIdsList", employeeIdsList);
        params.put("employeeIdsArray", employeeIdsArray);
        List<Employees> result = employeesMapper.getEmployeesMapParams(params);
        assertEquals(3, result.size());
    }
