# Python简单练习（十五）

- 家具类（HouseItem）具有名称和占地面积属性，其中：

  - 席梦思（bed）占地 4 平米

  - 衣柜（chest）占地 2 平米

  - 餐桌（table）占地 1.5 平米

- 房子类（House）具有户型、总面积、剩余面积和家具名称列表属性：
  - 新房子没有任何家具
  - 将家具的名称追加到家具名称列表中
  - 判断家具的面积是否超过剩余面积，如果超过，则提示不能添加这件家具

a. 将以上三件家具对象添加到房子对象中

b. 打印房子时，要求输出：户型、总面积、剩余面积、家具名称列表

**使用面向对象思想，编码完成上述功能。**

```python
# 家具类(HouseItem)
class HouseItem:
    __slots__ = ("name", "space")

    def __init__(self, name, space):
        self.name = name
        self.space = space

    def __str__(self):
        return f"{self.name}：占地面积{self.space}平方"

    __repr__ = __str__


# 房子类(House)
class House:
    # 限制属性动态绑定
    __slots__ = ("house_type", "all_area", "residual_area", "house_item_list")

    # 实例化对象
    def __init__(self, house_type, all_area, residual_area, house_item_list):
        self.house_type = house_type
        self.all_area = all_area
        self.residual_area = residual_area
        self.house_item_list = house_item_list

    # 重写父类__str__方法
    def __str__(self):
        return (f"户型为：{self.house_type}，总面积为：{self.all_area}平米，"
                f"剩余面积为：{self.residual_area}平米，家具名称列表为：{self.house_item_list}")

    # 添加家具方法
    def add_item(self, house_item):
        if house_item.space <= self.residual_area:
            self.house_item_list.append(house_item)
            self.residual_area -= house_item.space
            print(f"家具:{house_item.name}，已添加成功，占用面积：{house_item.space}平米")
        else:
            print(f"家具：{house_item.name}，无法添加，剩余空间不足")


if __name__ == '__main__':
    # 家具类 对象
    bed = HouseItem("席梦思", 4)
    chest = HouseItem("衣柜", 2)
    table = HouseItem("餐桌", 1.5)

    # 房子类 对象1
    house1 = House("汤臣一品", 200, 100, [])

    house1.add_item(bed)
    house1.add_item(chest)
    house1.add_item(table)

    print(house1)
    print()

    # 房子类 对象2
    house2 = House("出租屋", 10, 5, [])

    house2.add_item(bed)
    house2.add_item(chest)
    house2.add_item(table)

    print(house2)
```

