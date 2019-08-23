# 5.Swagger เพื่อคู่ API

💬 เวลาที่เรามี API หลายๆตัวละ เราก็จะเริ่มมีปัญหาว่าเรามี API อะไรบ้างและแต่ละตัวมันมี verb เป็นไรหรือมันใช้สำหรับทำอะไรบ้าง บลาๆ จากปัญหาที่ว่ามาในรอบนี้เราจะมารู้จักกับตัว **Swagger** ที่จะมาช่วยจัดกการกับ API ให้เราสบายขึ้น เช่น ทำ version หรือเขียน document

{% embed url="https://www.youtube.com/watch?v=HS1y1NNFC58&list=PLUjAn8nwWniheN3OLy6i7xu1VYq4Tzj7p&index=6" %}

## 🎯 สรุปสั้นๆ

### 👨‍🚀 ติดตั้ง Swaggers

{% embed url="https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle" %}

### 👨‍🚀 คำสั่งในการติดตั้ง

```text
dotnet add package Swashbuckle.AspNetCore
```

### 👨‍🚀 Configuration

```csharp
using Microsoft.OpenApi.Models;

public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddMvc()
        .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
    });
}

public void Configure(IApplicationBuilder app)
{
    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();

    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
    });

    app.UseMvc();
}
```

