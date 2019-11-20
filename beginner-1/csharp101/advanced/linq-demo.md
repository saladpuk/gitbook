---
description: "\U0001F914 คำสั่งของ LINQ ที่ได้ใช้บ่อยๆมีไรบ้างนะ"
---

# พระคัมภีร์การใช้คำสั่ง LINQ

บทความนี้เป็นบทความที่แยกออกมาจากเรื่อง LINQ ซึ่งเป็นหนึ่งในคำสั่งเทพเจ้าของสาย .NET ซึ่งมันจะทำให้ developer ทำงานได้สบายลงแบบฝุดๆ ดังนั้นใครยังไม่รู้เรื่อง LINQ ให้กลับไปอ่านบทความนี้ก่อนเน่อ [Saladpuk - LINQ 101](https://saladpuk.gitbook.io/learn/beginner-1/csharp101/advanced/linq)

## Filtering Data <a id="filtering-data-c"></a>

**Where** - เป็นการเลือกเอาเฉพาะข้อมูลที่เราสนใจออกมา เช่น มี data source เป็นเลข 1~100 แล้วต้องการเอาเฉพาะเลขที่ 5 และ 7 หารลงตัวออกมา ก็จะเขียนออกมาได้เป็นแบบนี้

```csharp
var collection = Enumerable.Range(1, 100);
var qry = collection.Where(it => it % 5 == 0 && it % 7 == 0);
// ผลลัพท์: { 35, 70 }
```

## Projection Operations

**Select** - เป็นการเลือกว่า data source ที่เราไปดึงข้อมูลมา เราจะดัดแปลงแก้ไข หรือ เลือกเอาเฉพาะข้อมูลบางส่วนออกมาใช้

เช่นมี collection เป็นเลข 1~5 ตอนที่เราจะเอามาทำงานด้วยเราจะแก้ให้มันถูก คูณด้วย 10 ก่อนค่อยเอามาใช้งาน ก็จะเขียนได้แบบนี้

```csharp
var collection = new int[] { 1, 2, 3, 4, 5 };
var qry = collection.Select(it => it * 10);
// ผลลัพท์: { 10, 20, 30, 40, 50 }
```

หรือ จะให้มันเปลี่ยนเป็นข้อมูลอีกประเภทนึงเลยก็ได้

```csharp
var qry = collection.Select(it => new Student{ Id = it });
```

```csharp
public class Student
{
    public int Id { get; set; }
}
```

ส่วนถ้าข้อมูลใน data source มันวุ่นวายเกินไป เราก็สามารถเลือกแค่บางส่วนของมันมาใช้ก็ได้นะ เช่น เราอยากได้แค่ Name ที่อยู่ใน collection มาใช้เท่านั้น ก็เขียนเป็นแบบนี้ได้

```csharp
var collection = new[]
{
    new { Id = 1, Name = "A", Age = 10 },
    new { Id = 2, Name = "B", Age = 15 },
    new { Id = 3, Name = "C", Age = 20 },
};
var qry = collection.Select(it => it.Name);
// ผลลัพท์: { "A", "B", "C" }
```

**SelectMany** - เป็นการเลือกเข้าไปถึงหน่วยย่อยของ collection ที่ซ้อนภายใน collection อีกทีนึง

```csharp
var collection = new[]
{
    new [] { 1, 2, 3, 4 },
    new [] { 5, 6, 7, 8 },
};
var qry = collection.SelectMany(it => it);
```

{% hint style="info" %}
**แนะนำให้อ่าน**  
คำสั่ง **SelectMany** สำหรับคนที่พึ่งหัดใช้ LINQ อาจจะ งงๆ หน่อยแต่ถ้าเราได้ทำงานร่วมกับพวก collection ซ้อน collection แล้วล่ะก็ควรจะทำความเข้าใจมันเอาไว้นะ ซึ่งอ่านได้จากลิงค์นี้เลย  
[Microsoft document - Projection Operations](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/projection-operations)
{% endhint %}

## Element Operations <a id="element-operations-c"></a>

ในบางทีเราอยากจะทำงานกับข้อมูลแค่ตัวใดตัวหนึ่งหรือส่วนหนึ่งที่อยู่ใน collection เราก็สามารถใช้คำสั่งที่อยู่ด้านล่างได้ เช่น เรามี data source ที่มีข้อมูลเป็นเลข 1~100

```csharp
var collection = Enumerable.Range(1, 100);
```

**First** - เอาเฉพาะตัวแรกออกมา

```csharp
var result = collection.First();
// ผลลัพท์: { 1 }
```

**FirstOrDefault** - เหมือนกับ First ทุกประการ ต่างกันแค่ถ้ามันดึงค่าออกมาไม่ได้มันจะส่งค่า default ของ data type นั้นๆกลับมา

```csharp
var result = collection.FirstOrDefault();
// ผลลัพท์: { 1 }
```

**Last** - เอาเฉพาะตัวสุดท้ายออกมา

```csharp
var result = collection.Last();
// ผลลัพท์: { 100 }
```

**LastOrDefault** - เหมือนกับ Last ทุกประการ ต่างกันแค่ถ้ามันดึงค่าออกมาไม่ได้มันจะส่งค่า default ของ data type นั้นๆกลับมา

```csharp
var result = collection.LastOrDefault();
// ผลลัพท์: { 100 }
```

**ElementAt** - เป็นการดึงค่าที่อยู่ใน index ที่กำหนดออกมา

```csharp
var result = collection.ElementAt(3);
// ผลลัพท์: { 4 }
```

**ElementAtOrDefault** - เหมือนกับ ElementAt ทุกประการ ต่างกันแค่ถ้ามันดึงค่าออกมาไม่ได้มันจะส่งค่า default ของ data type นั้นๆกลับมา

```csharp
var result = collection.ElementAtOrDefault(9999);
// ผลลัพท์: { 0 }
```

{% hint style="danger" %}
**อันตราย**  
ถ้า data source เป็น collection ว่าง แล้วไปใช้คำสั่งพวก **First, Last, ElementAt** มันจะทำให้เกิด Exception ได้ครับ ดังนั้นโดยปรกติผมจะแนะนำให้ใช้คำสั่ง **FirstOrDefault, LastOrDefault, ElementAtOrDefault** แทนมากกว่า เพราะค่า overhead ในการจัดการกับ error มันสูงกว่าครับ
{% endhint %}

## Partitioning Data <a id="partitioning-data-c"></a>

เวลาที่เราทำงานกับ data source ปริมาณมากๆ เราสามารถที่จะทำการแบ่งข้อมูลออกเป็นส่วนๆ เพื่อให้ง่ายในการทำงานได้ เช่น มี collection ตัวเลข 1~100 อยู่ตามด้านล่าง

```csharp
var collection = Enumerable.Range(1, 100);
```

**Take** - เป็นการสั่งให้ดึงข้อมูลจาก data source ออกมาเท่าที่เรากำหนดไว้ เช่น เราอยากดึงข้อมูลมาแค่ 5 ตัวแรกก่อน เราก็จะเขียนได้ว่า

```csharp
var qry = collection.Take(5);
// ผลลัพท์: { 1, 2, 3, 4, 5 }
```

**TakeLast** - เหมือนกับ Take แต่จะดึงมาจากด้านหลังสุด เช่น อยากจะดึงข้อมูล 5 ตัวจากด้านหลังสุดออกมา

```csharp
var qry = collection.TakeLast(5);
// ผลลัพท์: { 96, 97, 98, 99, 100 }
```

**TakeWhile** - เป็นการสั่งให้มันดึงข้อมูลจาก data source ออกมาเรื่อยๆจนกว่าจะเจอตัวแรกที่ทำให้เงื่อนไขไม่เป็นจริง เช่น ให้ดึงมาเรื่อยๆถ้าเลขที่ดึงมามันยังน้อยกว่า 8

```csharp
var qry = collection.TakeWhile(it => it < 8);
// ผลลัพท์: { 1, 2, 3, 4, 5, 6, 7 }
```

**Skip** - สั่งให้ข้ามข้อมูลเท่ากับที่เรากำหนด เช่น เราต้องการข้ามข้อมูล 4 ตัวแรกไป

```csharp
var qry = collection.Skip(4);
// ผลลัพท์: { 5, 6, 7 ... 100 }
```

**SkipLast** - เหมือนกับ Skip ต่างกันแค่มันจะข้ามเฉพาะตัวด้านหลังสุด เช่น อยากจะข้ามข้อมูล 4 ตัวสุดท้ายไป

```csharp
var qry = collection.SkipLast(4);
// ผลลัพท์: { 1, 2, 3 ... 96 }
```

**SkipWhile** - เป็นการสั่งให้มันข้ามข้อมูลไปเรื่อยๆ ถ้าเงื่อนไขยังเป็นจริงอยู่ และจะหยุดข้ามเมื่อเจอข้อมูลตัวแรกที่ไม่ตรงเงื่อนไข  เช่น อยากจะข้ามไปเรื่อยๆจนกว่าจะเจอตัวแรกที่มากกว่า 50

```csharp
var qry = collection.SkipWhile(it => it < 50);
// ผลลัพท์: { 50, 51, 52 ... 100 }
```

## Set Operations

เราสามารถทำงานกับ data source ที่เป็น 2 กลุ่มให้มาทำงานร่วมกันได้ 3 แบบคือ

![](../../../.gitbook/assets/image%20%28301%29.png)

เช่นเรามีข้อมูลกลุ่ม a กับกลุ่ม b เป็นแบบนี้

```csharp
var a = new[] { 1, 2, 3, 4, 5 };
var b = new[] { 4, 5, 6, 7, 8 };
```

**Intersect** - ตามรูปเลยคือ เอาเฉพาะที่มันเหมือนกันออกมา

```csharp
var intersect = a.Intersect(b);
// ผลลัพท์: { 4, 5 }
```

**Union** - ตามรูปเลยคือ เอาทั้งสองกลุ่มมารวมกัน

```csharp
var union = a.Union(b);
// ผลลัพท์: { 1, 2, 3, 4, 5, 6, 7, 8 }
```

**Except** - ตามรูปเลยคือ เอาเฉพาะของที่ไม่ซ้ำกับอีกกลุ่มออกมา

```csharp
var except = a.Except(b);
// ผลลัพท์: { 1, 2, 3 }
```

**Distinct** - เป็นการตัดตัวซ้ำทิ้ง

```csharp
var collection = new[] { 1, 1, 2, 2, 3, 4, 4, 3 };
var qry = collection.Distinct();
// ผลลัพท์: { 1, 2, 3, 4 }
```

## Sorting Data

การเรียงลำดับเราทำได้ 3 แบบ `น้อยไปมาก` `มากไปน้อย` และ `กลับด้านข้อมูล` เช่นเรามี data source เป็นแบบนี้

```csharp
var collection = new int[] { 7, 5, 2, 6, 4, 1, 3 };
```

**OrderBy** - เรียงลำดับจากน้อยไปมาก หรือถ้าเป็นตัวอักษรจะเป็นการเรียงจาก a~Z

```csharp
var ascending = collection.OrderBy(it => it);
// ผลลัพท์: { 1, 2, 3, 4, 5, 6, 7 }
```

**OrderByDescending** - เรียงลำดับจากมากไปน้อย

```csharp
var descending = collection.OrderByDescending(it => it);
// ผลลัพท์: { 7, 6, 5, 4, 3, 2, 1 }
```

**Reverse** - เรียงลำดับแบบกลับด้าน ขวาไปซ้าย แทน

```csharp
var reverse = collection.Reverse();
// ผลลัพท์: { 3, 1, 4, 6, 2, 5, 7 }
```

**ThenBy** และ **ThenByDescending** - แต่ถ้าข้อมูลมีความซับซ้อนมากขึ้น เราสามารถกำหนดความสำคัญในการเรียงลำดับได้ด้วย เช่น เรียงลำดับจากคะแนนน้อยไปมาก แต่ถ้าคะแนนเท่ากันให้เรียงจากชื่อตามลำดับตัวอักษรก็จะเขียนแบบนี้ได้

```csharp
var collection = new[]
{
    new { Score = 7, Name = "B" },
    new { Score = 3, Name = "A" },
    new { Score = 7, Name = "A" },
    new { Score = 4, Name = "A" },
    new { Score = 3, Name = "C" },
};

// น้อยไปมาก และ ตามลำดับตัวอักษร
var ascending = collection
                .OrderBy(it => it.Score)
                .ThenBy(it => it.Name);
                
// มากไปน้อย และ ตามลำดับตัวอักษร
var descending = collection
                .OrderByDescending(it => it)
                .ThenBy(it => it.Name);
```

## Quantifier Operations <a id="quantifier-operations-c"></a>

เราสามารถหาผลลัพท์จากข้อมูลใน collection ได้เช่น **มีบางตัวไหม?** หรือ **ทุกตัวเป็นแบบนี้ไหม?** อาจจะฟังแล้ว งงๆ ไปดูตัวอย่างเลยดีกว่า โดยสมมุติว่าผมมี data source เป็นแบบนี้

```csharp
var collection = new int[] { 2, 4, 6, 8, 10 };
```

**Any** - ถามว่ามีซักตัวไหมที่เป็นแบบนี้ เช่น อยากรู้ว่ามีซักตัวไหมใน collection ที่มีค่ามากกว่า 9 ก็สามารถเขียนเป็น

```csharp
var any = collection.Any(it => it > 9); // true
```

**All** - ถามว่าทุกตัวเป็นแบบนี้หรือเปล่า เช่น อยากเช็คว่าทุกตัวใน collection มากกว่า 5 หรือเปล่า

```csharp
var all = collection.All(it => it > 5); // false
```

**Contains** - ถามว่าภายในนั้นมีตัวนี้อยู่หรือเปล่า เช่น collection นั้นมีเลข 8 อยู่ในนั้นหรือเปล่า

```csharp
var contain = collection.Contains(8); // true
```

## Grouping Data <a id="grouping-data-c"></a>

**GroupBy** - สั่งให้มันจัดกลุ่มของข้อมูลได้ เช่น มี collection ของคนหลายๆคน แล้วเราอยากให้จัดกลุ่มคนตามอายุ เราก็จะเขียนได้ว่า

```csharp
var collection = new[]
{
    new { Name = "A", Age = 15 },
    new { Name = "B", Age = 7 },
    new { Name = "C", Age = 7 },
    new { Name = "D", Age = 15 },
    new { Name = "E", Age = 9 },
};
var qry = collection.GroupBy(it => it.Age);

/* ผลลัพท์
15
{ Name = A, Age = 15 }
{ Name = D, Age = 15 }
7
{ Name = B, Age = 7 }
{ Name = C, Age = 7 }
9
{ Name = E, Age = 9 }
*/
```

## Generation Operations <a id="generation-operations-c"></a>

ถ้าเราต้องการสร้างข้อมูลที่เป็น collection ขึ้นมาแบบง่ายๆ เราก็สามารถใช้ LINQ ช่วยสร้างได้

**Range** - สร้างชุดตัวเลขออกมา เช่น อยากได้ collection ตัวเลขตั้งแต่ 1 ถึง 100

```csharp
var qry = Enumerable.Range(1, 100);
// ผลลัพท์: { 1, 2, 3 ... 100 }
```

**Empty** - สร้าง collection ว่างออกมา เช่น เราอยากได้ collection ของตัวเลข แต่ไม่ต้องมีข้อมูลอะไรอยู่ข้างในนะ 

```csharp
var qry = Enumerable.Empty<int>();
// ผลลัพท์: { }
```

**DefaultIfEmpty** - ถ้าเราต้องไปทำงานกับ collection ตัวเลขซักตัว แต่ถ้า collection นั้นมันเป็นค่าว่าง เราจะกำหนดค่า 9 ให้มันไปใช้แทน

```csharp
var collection = Enumerable.Empty<int>();
var qry = collection.DefaultIfEmpty(9);
// ผลลัพท์: { 9 }
```

**Repeat** - สร้างชุดข้อมูลซ้ำๆกันออกมา เช่น อยากได้ collection เลข 5 ซ้ำกัน 3 ตัว ก็เขียนแบบนี้ได้

```csharp
var qry = Enumerable.Repeat(5, 3);
// ผลลัพท์: { 5, 5, 5 }
```

## Converting Data Types <a id="converting-data-types-c"></a>

เราสามารถแปลง data source ของเราจาก data type นึงไปยังอีก data type นึงก็ได้นะ เช่น มีข้อมูล collection เลข 1~5 ตามนี้

```csharp
var collection = new[] { 1, 2, 3, 4, 5 };
```

**AsEnumerable** - แปลงให้มันกลับมาเป็น IEnumerable&lt;T&gt; เอาไว้ช่วยแปลงจาก collection อะไรก็ตามให้กลับมาสู่ base class ของกลุ่ม collection

```csharp
var qry = collection.AsEnumerable();
```

**AsQueryable** - แปลงให้คำสั่งทั้งหมดยังเป็นแค่ Query เท่านั้น ซึ่งใช้ได้ดีตอนที่ทำงานร่วมกับ database เพราะเราจะได้ส่งแต่คำสั่งไปประมวลผลที่ database เท่านั้นไม่ได้ส่งข้อมูลปริมาณมหาศาลกลับมาถล่มที่ client 

```csharp
var qry = collection.AsQueryable();
```

**Cast** - แปลงข้อมูลจาก data source ให้กลายเป็น data type ที่กำหนด

```csharp
var collection = new[]
{
    new Dog { Id = 1, OwnerName = "Saladpuk" },
};
IEnumerable<Animal> qry = collection.Cast<Animal>();
```

```csharp
public class Animal
{
    public int Id { get; set; }
}
public class Dog : Animal
{
    public string OwnerName { get; set; }
}
```

**OfType** - เลือกเอาเฉพาะ data type ที่ตรงกับที่กำหนด

```csharp
var collection = new object[]
{
    new Dog { OwnerName = "Saladpuk" },
    new Cat { IsFriendly = false },
};
var qry = collection.OfType<Dog>();
// ผลลัพท์: [ { OwnerName = 'Saladpuk' } ]
```

```csharp
public class Dog
{
    public string OwnerName { get; set; }
}
public class Cat
{
    public bool IsFriendly { get; set; }
}
```

**ToArray** - แปลงให้ collection นั้นๆกลายเป็น Array

```csharp
var qry = Enumerable.Range(1, 100);
int[] result = qry.ToArray();
```

**ToList** - แปลงให้ collection นั้นๆกลายเป็น List

```csharp
var qry = Enumerable.Range(1, 100);
List<int> result = qry.ToList();
```

**ToDictionary** - แปลงให้ collection นั้นๆกลายเป็น Dictionary&lt;K, V&gt; เช่นทำการจัดกลุ่มว่าใครเรียนอยู่ห้องไหนบ้าง แล้วทำการไปสร้างเป็น dictionary

```csharp
var collection = new[]
{
    new { Id = 1, ClassRoom = "A", Name = "Saladpuk" },
    new { Id = 2, ClassRoom = "B", Name = "Thaksin" },
    new { Id = 3, ClassRoom = "C", Name = "Prayut" },
    new { Id = 4, ClassRoom = "B", Name = "Yingluck" },
    new { Id = 5, ClassRoom = "C", Name = "Abhisit" },
};
var result = collection
    .GroupBy(it => it.ClassRoom)
    .ToDictionary(it => it.Key, it => it);

/* ผลลัพท์
A
{ Id = 1, ClassRoom = A, Name = Saladpuk }
B
{ Id = 2, ClassRoom = B, Name = Thaksin }
{ Id = 4, ClassRoom = B, Name = Yingluck }
C
{ Id = 3, ClassRoom = C, Name = Prayut }
{ Id = 5, ClassRoom = C, Name = Abhisit }
*/
```

## Concatenation Operations <a id="concatenation-operations-c"></a>

**Concate** - เป็นการเอา 2 collection มาต่อกันแบบดื้อๆเลย

```csharp
var a = new[] { 1, 2, 3, 4, 5 };
var b = new[] { 4, 5, 6, 7, 8 };
var qry = a.Concat(b);
// ผลลัพท์: { 1, 2, 3, 4, 5, 4, 5, 6, 7, 8 }
```

## Aggregation Operations <a id="aggregation-operations-c"></a>

เป็นกลุ่มคำสั่งที่ได้ผลลัพท์กลับมาเลย และเป็นการทำงานแบบ **Imperative** เช่นมี data source เป็นเลข 1~10 ตามนี้

```csharp
var collection = Enumerable.Range(1, 10);
```

**Sum** - หาผลรวม

```csharp
var result = collection.Sum();
// ผลลัพท์: 55
```

**Average** - หาค่าเฉลี่ย

```csharp
var result = collection.Average();
// ผลลัพท์: 5.5
```

**Max** - หาค่าสูงสุด

```csharp
var result = collection.Max();
// ผลลัพท์: 10
```

**Min** - หาค่าต่ำสุด

```csharp
var result = collection.Min();
// ผลลัพท์: 1
```

**Count** - นับว่าภายใน data source มีข้อมูลอยู่ทั้งหมดเท่าไหร่

```csharp
var result = collection.Count();
// ผลลัพท์: 10
```

**Aggregate** - นำข้อมูลทั้ง collection มาดำเนินการแบบต่อเนื่องกัน

```csharp
var result = collection.Aggregate((a, b) => a * b);
// ผลลัพท์: 3628800
```

{% hint style="info" %}
**แนะนำให้อ่าน**  
คำสั่ง Aggregate ถ้าเราใช้เป็นจริงๆมันทรงพลังมากเลยนะ ลองศึกษาเพิ่มเติมได้จากลิงค์นี้เบย  
[Microsoft document - Aggregation](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.aggregate?view=netcore-3.0)
{% endhint %}

## บทสรุป Deferred vs Imperative

จากคำสั่งทั้งหมดที่เขียนมาเป็นตัวอย่าง สุดท้ายการทำงานของมันก็จะตกมาอยู่ในกลุ่ม 3 กลุ่มนั่นเองคือ 

* ทำงานโดยทันที **Immediate**
* ไม่ทำงานจนกว่าจะเรียกใช้ **Deferred**
  * ดึงข้อมูลทั้งหมดมาก่อนค่อยทำงาน **Non-Streaming**
  * ค่อยทะยอยดึงข้อมูลมาเรื่อยๆ **Streaming**

| Operators | Return Type | Immediate | Deferred Streaming | Deferred Non-Streaming |
| :--- | :--- | :--- | :--- | :--- |
| [Aggregate](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.aggregate) | TSource | X |  |  |
| [All](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.all) | [Boolean](https://docs.microsoft.com/en-us/dotnet/api/system.boolean) | X |  |  |
| [Any](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.any) | [Boolean](https://docs.microsoft.com/en-us/dotnet/api/system.boolean) | X |  |  |
| [AsEnumerable](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.asenumerable) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Average](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.average) | Single numeric value | X |  |  |
| [Cast](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.cast) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Concat](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.concat) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Contains](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.contains) | [Boolean](https://docs.microsoft.com/en-us/dotnet/api/system.boolean) | X |  |  |
| [Count](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.count) | [Int32](https://docs.microsoft.com/en-us/dotnet/api/system.int32) | X |  |  |
| [DefaultIfEmpty](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.defaultifempty) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Distinct](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.distinct) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [ElementAt](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.elementat) | TSource | X |  |  |
| [ElementAtOrDefault](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.elementatordefault) | TSource | X |  |  |
| [Empty](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.empty) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) | X |  |  |
| [Except](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.except) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X | X |
| [First](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.first) | TSource | X |  |  |
| [FirstOrDefault](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.firstordefault) | TSource | X |  |  |
| [GroupBy](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.groupby) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  |  | X |
| [GroupJoin](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.groupjoin) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X | X |
| [Intersect](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.intersect) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X | X |
| [Join](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.join) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X | X |
| [Last](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.last) | TSource | X |  |  |
| [LastOrDefault](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.lastordefault) | TSource | X |  |  |
| [LongCount](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.longcount) | [Int64](https://docs.microsoft.com/en-us/dotnet/api/system.int64) | X |  |  |
| [Max](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.max) | Single numeric value, TSource, or TResult | X |  |  |
| [Min](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.min) | Single numeric value, TSource, or TResult | X |  |  |
| [OfType](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.oftype) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [OrderBy](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.orderby) | [IOrderedEnumerable&lt;TElement&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.linq.iorderedenumerable-1) |  |  | X |
| [OrderByDescending](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.orderbydescending) | [IOrderedEnumerable&lt;TElement&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.linq.iorderedenumerable-1) |  |  | X |
| [Range](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.range) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Repeat](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.repeat) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Reverse](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.reverse) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  |  | X |
| [Select](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [SelectMany](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.selectmany) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [SequenceEqual](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.sequenceequal) | [Boolean](https://docs.microsoft.com/en-us/dotnet/api/system.boolean) | X |  |  |
| [Single](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.single) | TSource | X |  |  |
| [SingleOrDefault](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.singleordefault) | TSource | X |  |  |
| [Skip](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.skip) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [SkipWhile](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.skipwhile) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Sum](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.sum) | Single numeric value | X |  |  |
| [Take](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.take) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [TakeWhile](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.takewhile) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [ThenBy](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.thenby) | [IOrderedEnumerable&lt;TElement&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.linq.iorderedenumerable-1) |  |  | X |
| [ThenByDescending](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.thenbydescending) | [IOrderedEnumerable&lt;TElement&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.linq.iorderedenumerable-1) |  |  | X |
| [ToArray](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.toarray) | TSource array | X |  |  |
| [ToDictionary](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.todictionary) | [Dictionary&lt;TKey,TValue&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2) | X |  |  |
| [ToList](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.tolist) | [IList&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ilist-1) | X |  |  |
| [ToLookup](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.tolookup) | [ILookup&lt;TKey,TElement&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.linq.ilookup-2) | X |  |  |
| [Union](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.union) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |
| [Where](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.where) | [IEnumerable&lt;T&gt;](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) |  | X |  |



