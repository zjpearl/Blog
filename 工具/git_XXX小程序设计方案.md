## 数据库模型设计（MongoDB）

### User 用户类

| #  | 字段名     | 名称     | 字段类型    | 是否自动生成 | 初始值       | 功能                   |
|----|------------|----------|-------------|--------------|--------------|------------------------|
| 1  | **ID**     | -        | ObjectId    | 是           | -            | **全局**唯一标识用户   |
| 2  | CreatedAt  | 创建时间 | Time        | 是           | 创建时间     | -                      |
| 3  | UpdatedAt  | 修改时间 | Time        | 是           | CreatedAt    | -                      |
| 4  | DeletedAt  | 移除时间 | Time        | 是           | nil          | 标识用户是否被移除     |
| 5  | **OpenID** | -        | string      | 否           | 用户微信数据 | **小程序**唯一标识用户 |
| 6  | Nickname   | 昵称     | string      | 否           | 用户微信数据 | -                      |
| 7  | Gender     | 性别     | int         | 否           | 用户微信数据 | -                      |
| 8  | Age        | 年龄     | int         | 否           | 0            | -                      |
| 9  | Tags       | 标签     | []string    | 是           | []string{}   | 用户标签列表           |
| 10 | Friends    | 好友     | []ObjectId  | 是           | []ObjectId{} | 用户好友列表           |
| 11 | Groups     | 群组     | []ObjectId  | 是           | []ObjectId{} | 用户群组列表           |
| 12 | GSnapshot  | 花园快照 | []GSnapshot | 是           | []GSnapshot  | 用户往期**花园快照**   |

### GSnapshot 花园快照类
| #    | 字段名    | 名称     | 字段类型 | 是否自动生成 | 初始值    | 功能           |
| ---- | --------- | -------- | -------- | ------------ | --------- | -------------- |
| 1    | **ID**    | -        | ObjectId | 是           | -         | -              |
| 2    | CreatedAt | 创建时间 | Time     | 是           | 创建时间  | -              |
| 3    | UpdatedAt | 修改时间 | Time     | 是           | CreatedAt | -              |
| 4    | DeletedAt | 移除时间 | Time     | 是           | nil       | -              |
| 5    | Year      | 年       | int      | 是           | Date      | -              |
| 6    | Month     | 月       | int      | 是           | Date      | -              |
| 7    | Level     | 等级     | int      | 是           | 系统      | 该周期花园等级 |
| 8    |           |          |          |              |           |                |


### Garden 花园类

| #    | 字段名    | 名称     | 字段类型 | 是否自动生成 | 初始值     | 功能         |
| ---- | --------- | -------- | -------- | ------------ | ---------- | ------------ |
| 1    | **ID**    | -        | ObjectId | 是           | -          | -            |
| 2    | CreatedAt | 创建时间 | Time     | 是           | 创建时间   | -            |
| 3    | UpdatedAt | 修改时间 | Time     | 是           | CreatedAt  | -            |
| 4    | DeletedAt | 移除时间 | Time     | 是           | nil        | -            |
| 6    | UID       | 用户ID   | ObjectId | 是           | -          | 标识所属用户 |
| 7    | Coin      | 金币     | int      | 是           | 0          | **金币**值7  |
| 8    | Dew       | 雨露     | int      | 是           | 0          | **雨露**值   |
| 9    | Level     | 等级     | int      | 是           | 1          | 花园等级     |
| 10   | Flowers   | 花朵     | []Flower | 是           | []Flower{} | 花园花朵信息 |
|      |           |          |          |              |            |              |

#### Flower 花朵结构体

| # | 字段名     | 名称   | 字段类型 | 功能         |
|---|------------|--------|----------|--------------|
| 1 | ID         | 种子ID | ObjectId | 标识种子     |
| 2 | Level      | 等级   | int      | -            |
| 3 | Coordinate | 坐标   | []int    | 种植的坐标   |
| 4 | Dew        | 雨露   | int      | 拥有的雨露值 |

### Seed 种子类

| # | 字段名      | 名称             | 字段类型 | 是否自动生成 | 初始值    | 功能                 |
|---|-------------|------------------|----------|--------------|-----------|----------------------|
| 1 | **ID**      | -                | ObjectId | 是           | -         | -                    |
| 2 | CreatedAt   | 创建时间         | Time     | 是           | 创建时间  | -                    |
| 3 | UpdatedAt   | 修改时间         | Time     | 是           | CreatedAt | -                    |
| 4 | DeletedAt   | 移除时间         | Time     | 是           | nil       | 标识种子是否被移除   |
| 5 | Name        | 名称             | string   | 否           | -         | 种子名称             |
| 6 | Description | 描述             | string   | 否           | 自定      | 种子描述             |
| 7 | ReqDew      | 各等级所需雨露值 | []int    | 否           | 自定      | 种子开花所需的雨露值 |
| 8 | SeedImage   | 各等级种子图片   | []string | 否           | 自定      | 种子展示图片URL      |
| 9 | FlowerImage | 各等级花朵图片   | []string | 否           | 自定      | 花朵展示图片URL      |

## 缓存模型设计（Redis）

### User 用户类

| 名称         | Key        | Value Type | Value  |
|--------------|------------|------------|--------|
| 用户学习计时 | time：id   | string     | Date() |
| 用户签到时间 | signin：id | hash       | Date() |

**广场**

1. 动态

2. 点击头像进入个人主页加好友

   ![1581578969780](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581578969780.png)

**学习**





**花园**



**我**



