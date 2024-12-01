根据您提供的宿舍管理系统E-R图描述，我们可以将这些实体和它们之间的关系转换为关系数据库中的表结构。以下是每个实体的关系模式以及它们之间的联系的详细描述：

### 1. 实体及属性转关系模式

#### 宿舍 (Dormitory)
- `dormitory_id` (INT, PRIMARY KEY) - 宿舍编号
- `droom_type` (VARCHAR) - 宿舍类型（如4人间、6人间）
- `floor` (INT) - 所在楼层
- `capacity` (INT) - 可容纳人数

#### 学生 (Student)
- `student_id` (INT, PRIMARY KEY) - 学号
- `name` (VARCHAR) - 姓名
- `gender` (CHAR(1)) - 性别
- `age` (INT) - 年龄
- `class` (VARCHAR) - 班级
- `contact_info` (VARCHAR) - 联系方式

#### 宿管人员 (DormManager)
- `manager_id` (INT, PRIMARY KEY) - 工号
- `name` (VARCHAR) - 姓名
- `gender` (CHAR(1)) - 性别
- `age` (INT) - 年龄
- `contact_info` (VARCHAR) - 联系方式
- `shift_time` (VARCHAR) - 值班时间

#### 访客 (Visitor)
- `visitor_id` (INT, PRIMARY KEY) - 访客编号
- `name` (VARCHAR) - 姓名
- `gender` (CHAR(1)) - 性别
- `contact_info` (VARCHAR) - 联系方式
- `reason_for_visit` (VARCHAR) - 来访事由
- `id_card` (VARCHAR) - 身份证号

#### 设施 (Facility)
- `facility_id` (INT, PRIMARY KEY) - 设施编号
- `facility_name` (VARCHAR) - 设施名称（如床、书桌、衣柜）
- `status` (VARCHAR) - 设施状态（正常、损坏）
- `purchase_date` (DATE) - 购买日期

#### 维修记录 (MaintenanceRecord)
- `maintenance_id` (INT, PRIMARY KEY) - 维修编号
- `maintenance_date` (DATE) - 维修日期
- `content` (TEXT) - 维修内容
- `cost` (DECIMAL) - 维修费用
- `manager_id` (INT, FOREIGN KEY REFERENCES DormManager(manager_id)) - 维修人员工号

#### 缴费记录 (PaymentRecord)
- `payment_id` (INT, PRIMARY KEY) - 缴费编号
- `payment_date` (DATE) - 缴费日期
- `amount` (DECIMAL) - 缴费金额
- `type` (VARCHAR) - 缴费类型（住宿费、水电费等）
- `student_id` (INT, FOREIGN KEY REFERENCES Student(student_id)) - 学号

#### 宿舍分配记录 (DormAllocation)
- `allocation_id` (INT, PRIMARY KEY) - 分配编号
- `allocation_date` (DATE) - 分配日期
- `student_id` (INT, FOREIGN KEY REFERENCES Student(student_id)) - 学号
- `dormitory_id` (INT, FOREIGN KEY REFERENCES Dormitory(dormitory_id)) - 宿舍编号
- `status` (VARCHAR) - 分配状态（如已入住、待入住等）

#### 投诉建议 (Complaint)
- `complaint_id` (INT, PRIMARY KEY) - 投诉编号
- `complaint_date` (DATE) - 投诉日期
- `content` (TEXT) - 内容
- `status` (VARCHAR) - 处理状态
- `student_id` (INT, FOREIGN KEY REFERENCES Student(student_id)) - 学号

#### 通知公告 (Notice)
- `notice_id` (INT, PRIMARY KEY) - 公告编号
- `title` (VARCHAR) - 标题
- `publish_date` (DATE) - 发布日期
- `content` (TEXT) - 内容
- `manager_id` (INT, FOREIGN KEY REFERENCES DormManager(manager_id)) - 发布人员工号

### 2. 实体间关系转关系模式

对于一对一、一对多和多对一的关系，可以直接通过外键来表示。但对于多对多的关系，则需要创建额外的连接表。

#### 学生 - 宿舍 (多对一)
- 在 `DormAllocation` 表中已经包含了这个关系，不需要额外创建表。

#### 宿管人员 - 宿舍 (一对多)
- 在 `Dormitory` 表中添加一个 `manager_id` 字段作为外键。
  - `manager_id` (INT, FOREIGN KEY REFERENCES DormManager(manager_id))

#### 学生 - 访客 (多对多)
- 创建新的连接表 `Student_Visitor`：
  - `student_visitor_id` (INT, PRIMARY KEY)
  - `student_id` (INT, FOREIGN KEY REFERENCES Student(student_id))
  - `visitor_id` (INT, FOREIGN KEY REFERENCES Visitor(visitor_id))
  - `visit_time` (DATETIME) - 访问时间
  - `leave_time` (DATETIME) - 离开时间

#### 设施 - 宿舍 (多对一)
- 在 `Facility` 表中添加一个 `dormitory_id` 字段作为外键。
  - `dormitory_id` (INT, FOREIGN KEY REFERENCES Dormitory(dormitory_id))
  - `installation_date` (DATE) - 安装日期

#### 维修记录 - 设施 (多对一)
- 在 `MaintenanceRecord` 表中已经包含了这个关系，即 `facility_id` 作为外键。

#### 缴费记录 - 学生 (一对多)
- 在 `PaymentRecord` 表中已经包含了这个关系，即 `student_id` 作为外键。

#### 宿舍分配记录 - 学生、宿舍 (多对多、多对一)
- 在 `DormAllocation` 表中已经包含了这个关系，即 `student_id` 和 `dormitory_id` 作为外键，并且包含分配状态字段。

#### 投诉建议 - 学生 (多对一)
- 在 `Complaint` 表中已经包含了这个关系，即 `student_id` 作为外键。

#### 通知公告 - 宿管人员 (多对一)
- 在 `Notice` 表中已经包含了这个关系，即 `manager_id` 作为外键。

这样，我们就完成了从ER图到关系模式的转换。每个表都包含了必要的主键和外键约束，以确保数据的一致性和完整性。此外，还可以根据实际需求添加更多的检查约束和其他类型的约束。