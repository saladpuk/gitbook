---
description: "\U0001F914 เซิฟเวอร์ที่รับผู้ใช้เป็นล้านๆ หาได้จากไหนนะ ?"
---

# 👶 App Service Plan

ทุกวันนี้แม้แต่เด็กประถมก็สามารถมีเว็บไซต์เป็นของตัวเองได้ และไม่ว่าจะไปที่ไหนเราก็จะเห็น **Hosting** ให้ใช้กันอยู่เกลื่อนกลาดเลย มีทั้งฟรีบ้าง เสียเงินบ้าง แต่ถ้าถามว่ามันรองรับผู้ใช้หลักแสนหลักล้านคนได้หรือเปล่า? เวลาเซิฟเวอร์พังขึ้นมามีอะไรช่วยเราได้บ้าง? ดังนั้นในรอบนี้เราจะมาทำความรู้จักกับ **`App Service Plan`** ของ **Microsoft Azure** กันดูบ้าง ว่าเขามีไม้เด็ดอะไรให้เราเล่นบ้าง และ เมื่อเทียบกับ Hosting ทั่วไป มันจะเป็นยังไงบ้างน๊าา

{% hint style="success" %}
**แนะนำให้อ่าน**  
สำหรับคนที่ไม่รู้เรื่องคลาว์เลย แต่แอบมีใจอยากลองเด่ขาเข้ามาฝั่งนี้บ้าง ผมแนะนำให้ลองดูบทเรียนในลิงค์ด้านล่างนี้ก่อนนะ มันจะได้รู้ว่าควรจะสละเวลาเล่นเกมส์มาศึกษามันจริงๆหรือเป่านั่นเอง [**👶 Cloud พื้นฐาน**](https://www.saladpuk.com/basic/cloud101)
{% endhint %}

## 🤔 App Service Plan คือไย ?

![](../.gitbook/assets/image%20%28286%29%20%281%29.png)

จะมองว่ามันคือ **เซิฟเวอร์ฟาร์ม** ก็ได้ หรือพูดแบบบ้านๆก็คือ **มันคือกลุ่มของเซิฟเวอร์หลายๆตัวที่ทำงานร่วมกัน** ยังไงล่ะ โดยบางครั้งแอพของเรามันต้องไปทำงานอยู่บนเซิฟเวอร์ซักตัว เช่น **เว็บไซต์, Web API, Back-end** บลาๆ ดังนั้นเราก็เลยต้องไปใช้บริการพวก Hosting ต่างๆมาตั้งเป็นเซิฟเวอร์ใช้งานยังไงล่ะ หรือจะซื้อเซิฟเวอร์มาตั้งใช้งานเองในบริษัทไรงี้ ซึ่ง **คลาว์เซิฟเวอร์** ก็เป็นอีกหนึ่งในตัวเลือกนี้เช่นกัน และแน่นอนว่าทั้งหมดนี่ ก็สามารถทำบน **App Service Plan** ได้เลยนะฮ๊าฟ

## 🤔 ทำไยได้ ?

เย๊อะม๊วกกกก และเรียกได้เลยว่าตั้งแต่ผมใช้คลาว์มาตั้งแต่ปี 2010 ผมก็ไม่เคยใช้ Hosting อื่นๆอีกเลย เพราะเขามีทุกอย่างที่เราต้องการจริงๆ ดังนั้นมาลองไล่ดูกันทีละตัวเรยยยย

### 🔥 Azure Regions

เวลาที่เราตั้งเซิฟเวอร์บนคลาว์ เราสามารถเลือกได้เลยว่าจะให้เซิฟเวอร์อยู่ที่ไหน เพราะ **ยิ่งเซิฟเวอร์ยิ่งอยู่ใกล้ลูกค้าแค่ไหน ลูกค้าก็จะใช้งานเซิฟเวอร์ได้ไหลลื่นเท่านั้น** เรื่องนี้เป็นหนึ่งในข้อได้เปรียบของคลาว์ เพราะใน Hosting หลายๆที่ไม่สามารถทำเรื่องนี้ได้ และของ **Microsoft เป็นคลาว์แท้** ที่มี **Cloud Characteristics** ครบทั้ง 5 อย่าง หาใช่คลาว์เทียมที่มีอยู่เกลื่อนตลาดไม่

![https://azure.microsoft.com/en-us/global-infrastructure/regions](../.gitbook/assets/image%20%282%29%20%281%29.png)

{% hint style="info" %}
**เกร็ดความรู้**  
ข้อดีของการที่เราสามารถเลือกเซิฟเวอร์ได้หลายๆโซนคือ **ระบบจะสามารถป้องกัน Data Center ล่ม**ได้ เช่น เรามีเปิดเว็บโดยตั้งเซิฟเวอร์ไว้ที่ **ญี่ปุ่น** และ **เมกา** และด้วยเหตุผลอะไรก็ไม่รู้ โรงงานนิวเคลียญี่ปุ่นดันระเบิดขึ้น ทำให้ Data Center ของเขาใช้การไม่ได้เลย และแน่นอนว่าเซิฟเวอร์ก็ดับหมดในโซนญี่ปุ่น แต่ว่าลูกค้าเรามีอยู่ทั่วโลกก็ยังสามารถเข้าใช้งานเว็บเราได้ตามปรกติ เพราะเรามีเซิฟเวอร์อีกที่อยู่ใน เมกา นั่นเอง และด้วยลักษณะแบบนี้เราเลยสามารถทำ **Data center disaster recovery plan** ได้นั่นเอง ซึ่งพวก Hosting ทั่วไปหาตัวยากมากเลยที่จะทำแบบนี้ได้
{% endhint %}

### 🔥 Scaling

โดยทั่วไปเซิฟเวอร์ก็จะทำงานชิวๆไปวันๆ แต่ในบางครั้งก็อาจมีผู้ใช้โถมกระหน่ำเข้ามาใช้งานอย่างหักหน่วง ไม่ว่าจะเป็นเพราะ เทศการต่างๆ ไวรัสระบาด หรือ การโดนโจมตีจากผู้ไม่หวังดี บลาๆ ซึ่งแน่นอนถ้ามีการทำงานหนักเข้ามาเรื่อยๆก็จะทำให้เซิฟเวอร์ทำงานได้ไม่ทัน สุดท้ายก็จะอืดเป็นเต่าและล่มไปในที่สุด ... แต่หาใช่บนคลาว์ไม่ เพราะบนคลาว์เราสามารถ **`ขยายเครื่อง`** โดยการ **เพิ่ม** หรือ **ลด** ได้ดั่งใจ เพียงคลิกแค่ปลายนิ้ว โดยรูปแบบการขยายเครื่องบนคลาว์นั้นมี 2 รูปแบบนั่นคือ

#### ✨ Scale Up & Scale Down

เป็นการ เพิ่ม/ลด **ความแรงของเครื่อง** เช่น CPU, RAM, DISK ต่างๆ เพื่อเร่งให้เครื่องทำงานได้เร็วยิ่งขึ้นกว่าเดิม หรือรองรับการประมวลผลได้มากขึ้นนั่นเอง โดยการขยายในลักษณะนี้เราเรียกว่า **ขยายในแนวดิ่ง \(Vertical Scaling\)** นั่นเอง

![](../.gitbook/assets/image%20%2855%29%20%281%29.png)

#### ✨ Scale Out & Scale In

เป็นการ เพิ่ม/ลด **จำนวนเครื่อง** เช่น จากเดิมมีเซิฟเวอร์ 1 ตัวก็เพิ่มเป็น 5 ตัว ทำให้รับผู้ใช้งานได้มากกว่าเดิม 5 เท่า โดยการขยายในลักษณะนี้เราเรียกว่า **ขยายในแนวนอน \(Horizontal Scaling\)** นั่นเอง

![](../.gitbook/assets/image%20%281000%29.png)

#### ✨ Setting

และทั้งหมดนี่ถามว่าเราต้องมานั่งดูเซิฟเวอร์ตลอดเวลาเพื่อคอย เพิ่ม/ลด เซิฟเวอร์หรือเปล่า? คำตอบคือ ไม่ต้องครับ เพราะ Microsoft เขารู้ดีว่า เราก็ไม่อยากมานั่งทำแบบนั้นเหมือนกัน ดังนั้นทั้งหมดนี่เราสามารถตั้งได้เลยว่า จะขยายเพิ่มลดเซิฟเวอร์แบบไหน ซึ่งมีตัวเลือกให้เรา 2 แบบหลักคือ

![](../.gitbook/assets/image%20%28342%29%20%281%29.png)

* **Manual scale** - เป็นการกำหนดการขยายเซิฟเวอร์เอง เช่น เรารู้ว่าลูกค้าชอบแห่มาซื้อของช่วงจุดจีน ดังนั้นเราก็ตั้งไว้เลยว่า วันจุดจีนของทุกปีขอขยายเครื่องเป็น 5 เท่า แล้วพอเลยวันจุดจีนค่อยลดเครื่องกลับมาเหลือ 1 ตัวเป็นปรกติ
* **Custom auto scale** - เป็นการให้เซิฟเวอร์มันขยายตัวได้เองอัตโนมัติ โดยเราสามารถตั้งเงื่อนไขได้ เช่น ถ้า CPU มันเริ่มขึ้นเกิน 75% ก็ให้ทำการเพิ่มเครื่องอีก 1 ตัว และ ถ้ายังไม่พอก็เป็น 3 ตัว แต่ถ้า CPU กลับมาเป็นปรกติแล้ว ก็ค่อยลดเครื่องกลับมาเป็นปรกติก็ได้

{% hint style="success" %}
**แนะนำให้อ่าน**  
พอดีเขียนบทความเรื่อง Auto Scaling ไว้แล้ว ดังนั้นถ้าสนใจจุดนี้ก็ไปอ่านได้จากลิงค์นี้เบย [**Auto Scaling**](https://www.saladpuk.com/cloud/azure101/auto-scaling)
{% endhint %}

{% hint style="info" %}
**มุมชวนคิด**  
โดยปรกติถ้าเราต้องซื้อหรือเช่าเซิฟเวอร์มาใช้งานในบริษัทซักเครื่อง ถามว่าเบิกเงินซื้อยากไหมกว่าจะอนุมัติจนได้เครื่องมา? แล้วถ้าเอามาใช้ได้ซักพักพบว่าต้องการเครื่องเพิ่มอีก จะเบิกใหม่ได้ไหม? จะได้ทันเอามาใช้หรือเปล่า? และถ้าช่วงเทศการที่มีคนแห่มาใช้เซิฟเวอร์จนทำให้มันล่มบ่อยๆ เราจะขอเบิกเงินไปซื้อเครื่องมาเพิ่ม เพื่อมาใช้แค่ 2-3 วันได้หรือเปล่า? เครื่องที่ซื้อมาจะคืนได้ไหม? และอีกสารพัดปัญหาที่ขี้เกียจเขียน ... แต่ในทางกลับกัน ถ้าเราใช้เซิฟเวอร์บนคลาว์ เราอยากได้เท่าไหร่ก็กดได้เลย และถ้าไม่ใช้ก็ลดเครื่องลง เพียงแค่นี้มันก็ช่วยประหยัดเงินไปได้เยอะแล้วนะ เพราะเซิฟเวอร์ตัวนึงมันไม่จบแค่หลักหมื่น แถมยังไม่นับเวลาในการติดตั้ง ดูแลรักษา บลาๆเลยด้วยนะจ๊ะ 😘
{% endhint %}

### 🔥 Operating System

แน่นอนด้วยการที่ App Service Plan มันคือเซิฟเวอร์ที่เราจะใช้ทำงานด้วย ดังนั้นเราก็ย่อมเลือก OS ได้ด้วยเช่นกัน ซึ่งทาง Microsoft เขาก็มีให้เราเลือก 2 ค่ายแบบที่รู้ๆกันอยู่แล้วนั่นคือ **Windows** กับ **Linux** นั่นเองครัชชชช

![](../.gitbook/assets/image%20%28515%29%20%281%29.png)

### 🔥 Docker Containers

เทรน **Docker** นี้มาแรง และทาง Microsoft ก็รู้ดี ดังนั้นเซิฟเวอร์นี้ก็รองรับการ Deploy งานผ่าน Docker เช่นกันขอรับ

![](../.gitbook/assets/image%20%28977%29%20%281%29.png)

{% hint style="success" %}
**แนะนำให้อ่าน**  
สำหรับเพื่อนๆที่ยังไม่รู้ว่า Docker คืออะไร หรืออยากจะลองศึกษาใช้งาน ก็สามารถเข้าไปดูเพิ่มเติมได้จากลิงค์นี้เบยขอรับ [**👶 Docker ขั้นพื้นฐาน**](https://www.saladpuk.com/basic/docker)\*\*\*\*
{% endhint %}

### 🔥 Development Frameworks <a id="development-frameworks"></a>

รองรับภาษาอะไรบ้างนะเหรอ ตามลิสต์นี้เลย `.NET Framework`, `.NET Core`, `Java`, `Ruby`, `Node.js`, `PHP`, `Phyton` อีกทั้งเรายังเลือก version ย่อยได้อีกนะ ตามรูปด้านล่าง เวอร์ชั่นใหม่ๆออกก็กระโดดมาให้ใช้ทันที + ยังคงเวอร์ชันเก่าให้เลือกใช้ได้ยาวๆอีกด้วย

![](../.gitbook/assets/image%20%28331%29%20%281%29.png)

### 🔥 รวบรัดตัดจบ

คือถ้าจะให้มาไล่ความสามารถของเจ้าตัวนี้ทีละอย่าง บทความนี้ก็คงยาวจนผมหลับไปแน่นอน ดังนั้นผมขอตัดจบดื้อๆแบบนี้ไปก่อน แล้วเดี๋ยวจะกลับมาลงรายละเอียดทีละเรื่องของมันใน บทความย่อยของมันละกันฮั๊ฟ 😋 แต่สิ่งที่เราควรรู้สิ่งสุดท้ายคือ App Service Plan นั้นมีทั้งหมด 3 รูปแบบด้านล่าง ซึ่งแต่ละตัวเดี๋ยวจะลงรายละเอียดให้อีกทีเด้อ

#### ✨ Multi-tenant Service

แบบทั่วๆไปที่เราใช้กันอยู่นั่นแหละ ซึ่งมันจะอยู่ในรูปแบบแชร์ Network Infrastructure กัน และปรกติจะเปิดให้เข้าใช้งานผ่าน Internet ได้เลย

#### ✨ App Service Environment \(ASE\)

เป็นแบบที่เราอยากจะแยก Network ออกมาต่างหาก \(Network Isolation\) เพื่อต้องการปรับแต่งที่มากขึ้นกว่าปรกติ

#### ✨ Azure Stack

สำหรับคนที่อยากจะได้ความสามารถต่างๆของ Azure ไปทำงานในเซิฟเวอร์ตัวเอง \(On-Premise\)

## 🤔 แพงป่ะ ?

ร่ายมาซะขนาดนี้คิดว่าราคาเท่าไหร่กันเอ่ย? ... ฟรีจ้า ล้อเล่นนะ แต่มันมีตัวฟรีให้เราใช้งานพวกนี้จริงๆนะ ดังนั้นเดี๋ยวมาดูกันหน่อยว่า ถ้าเราสนใจอยากใช้เซิฟเวอร์ของ Microsoft Azure กันแล้ว เราจะต้องใช้ตัวไหน และ มันงบเท่าไหร่กันบ้าง

ทาง Microsoft เขาได้แบ่งเกรดของเซิฟเวอร์ออกตามลักษณะการใช้งานเป็น 5-6 ระดับคือ

| ชื่อระดับ | ลักษณะการใช้งาน |
| :--- | :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left">1.FREE</th>
      <th style="text-align:left">
        <p>&#xE25;&#xE2D;&#xE07;&#xE02;&#xE2D;&#xE07; &#xE25;&#xE2D;&#xE07;&#xE27;&#xE34;&#xE0A;&#xE32;
          &#xE01;&#xE47;&#xE41;&#xE04;&#xE48;&#xE2D;&#xE22;&#xE32;&#xE01;&#xE25;&#xE2D;&#xE07;&#xE2D;&#xE48;&#xE30;
          &#xE40;&#xE02;&#xE32;&#xE44;&#xE21;&#xE48;&#xE04;&#xE34;&#xE14;&#xE40;&#xE07;&#xE34;&#xE19;&#xE41;&#xE21;&#xE49;&#xE41;&#xE15;&#xE48;&#xE1A;&#xE32;&#xE17;&#xE40;&#xE14;&#xE35;&#xE22;&#xE27;</p>
        <p>&#xE41;&#xE15;&#xE48;&#xE01;&#xE47;&#xE21;&#xE35;&#xE02;&#xE49;&#xE2D;&#xE08;&#xE33;&#xE01;&#xE31;&#xE14;&#xE14;&#xE49;&#xE32;&#xE19;&#xE25;&#xE48;&#xE32;&#xE07;&#xE40;&#xE2B;&#xE21;&#xE37;&#xE2D;&#xE19;&#xE01;&#xE31;&#xE19;&#xE25;&#xE2D;&#xE07;&#xE2D;&#xE48;&#xE32;&#xE19;&#xE14;&#xE39;&#xE19;&#xE30;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<table>
  <thead>
    <tr>
      <th style="text-align:left">2.Shared</th>
      <th style="text-align:left">
        <p>&#xE17;&#xE33;&#xE2B;&#xE23;&#xE31;&#xE1A;&#xE43;&#xE0A;&#xE49;&#xE17;&#xE14;&#xE25;&#xE2D;&#xE07;&#xE01;&#xE48;&#xE2D;&#xE19;&#xE40;&#xE2D;&#xE32;&#xE44;&#xE1B;&#xE43;&#xE0A;&#xE49;&#xE07;&#xE32;&#xE19;&#xE08;&#xE23;&#xE34;&#xE07;
          &#xE2B;&#xE23;&#xE37;&#xE2D; &#xE40;&#xE2D;&#xE32;&#xE44;&#xE27;&#xE49;&#xE43;&#xE0A;&#xE49;&#xE2A;&#xE33;&#xE2B;&#xE23;&#xE31;&#xE1A;&#xE40;&#xE17;&#xE2A;&#xE23;&#xE30;&#xE1A;&#xE1A;</p>
        <p>&#xE42;&#xE14;&#xE22;&#xE08;&#xE30;&#xE43;&#xE0A;&#xE49;&#xE40;&#xE04;&#xE23;&#xE37;&#xE48;&#xE2D;&#xE07;&#xE41;&#xE1A;&#xE1A;&#xE41;&#xE0A;&#xE23;&#xE4C;&#xE17;&#xE23;&#xE31;&#xE1E;&#xE22;&#xE32;&#xE01;&#xE23;&#xE01;&#xE31;&#xE19;
          (&#xE44;&#xE21;&#xE48;&#xE21;&#xE35;&#xE43;&#xE19; Linux)</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

| 3.Basic | เหมือนระดับ 2 แต่เครื่องไม่ได้แชร์ทรัพยากรกันละ |
| :--- | :--- |


| 4.Standard | สำหรับใช้ทำงานจริง มีลูกค้าจริง มีเงินวิ่งในระบบจริง เครื่องไม่แชร์กัน |
| :--- | :--- |


| 5.Premium | เหมือนระดับ 3 แต่เครื่องจะแรงกว่า ขยายได้มากกว่า |
| :--- | :--- |


| 6.Isolated | เหมือนระดับ 4 แต่เขาจะแยก Network ออกมาให้เราโดยเฉพาะ |
| :--- | :--- |


![Windows](../.gitbook/assets/image%20%28159%29%20%281%29.png)

![Linux](../.gitbook/assets/image%20%2845%29%20%281%29.png)

{% hint style="success" %}
รายละเอียดของเซิฟเวอร์ทั้งหมด สามารถอ่านได้จากลิงค์ด้านล่างนี้ครัช และราคาของเซิฟเวอร์แต่ละตัวยังสามารถลดได้อีกนะขึ้นอยู่กับแต่ละ Subscription หรือสิทธิพิเศษของแต่ละคนที่ทาง Microsoft มอบให้  
[https://azure.microsoft.com/en-us/pricing/details/app-service/windows](https://azure.microsoft.com/en-us/pricing/details/app-service/windows/)
{% endhint %}

## 🎯 บทสรุป

ถ้าพูดถึงความสามารถของ **App Service Plan** เทียบกับ Hosting ทั่วไปในท้องตลาดก็คงไม่ต้องพูดถึง เพราะคลาว์แท้ย่อมชนะแทบจะทุกเรื่อง แต่ถ้ามาดูในแง่ของ **`ราคา`** แล้วจะพบว่า **ราคาท้องตลาดจะถูกกว่าบางเรื่อง** ดังนั้นเราเลยต้องมานั่งดูที่ภาพรวมว่า สิ่งที่เราได้มาจากการใช้คลาว์นั่นคือ **การรับมือกับผู้ใช้งานปริมาณมากๆ และ ความสามารถในการแก้ปัญหาที่สูงกว่า อีกทั้งเรื่อง Security ระดับโลก** แล้วล่ะก็ ผมว่ามันก็เหมาะกับดีนะ เพราะ สุดท้ายค่าบำรุงรักษาในการใช้ Hosting ทั่วไปอาจจะสูงกว่ามาใช้เซิฟเวอร์บนคลาว์ก็เป็นได้ แถมถ้าเป็นงานที่ต้องการคุณภาพสูงๆแล้วล่ะก็ จ่ายเพิ่มแค่เดือนละไม่กี่ร้อยบาท ผมว่ามันก็คุ้มค่าแล้วล่ะ เพราะเวลามันประเมิณค่าไม่ได้นั่นเอง

{% hint style="info" %}
คอร์สนี้กำลังค่อยๆเขียนอยู่ \(แต่คงจะกลับมาเขียนใหม่หลังจากเขียนคอร์ส [**👶 Azure App Services**](https://www.saladpuk.com/cloud/azure-app-services) เสร็จ\) ใครที่ไม่อยากพลาดอัพเดทก็เข้าไปกดติดตามที่ลิงค์ [**Mr.Saladpuk**](https://www.facebook.com/mr.saladpuk) ได้เลย ส่วนใครที่อยากศึกษา App Service ตัวไหนล่วงหน้าก็ส่งข้อความไปทักเอาได้เช่นกันขอรับ
{% endhint %}

{% hint style="success" %}
**แนะนำให้อ่าน**  
สำหรับคนใจร้อนอยากลองสร้างเว็บหรือเอาเว็บที่มีอยู่ไปใช้บน App Service Plan แล้วล่ก็สามารถทำตามได้จากลิงค์นี้เลยขอรับ [**สร้างเว็บตัวแรกกัน**](https://www.saladpuk.com/cloud/azure101/website) ส่วนลิงค์นี้เป็นการสร้าง Virtual Machine เผื่อสนใจศึกษาเพิ่มเติม [**สร้าง Virtual Machine กัน**](https://www.saladpuk.com/cloud/azure101/vm)\*\*\*\*
{% endhint %}

