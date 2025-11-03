
# Swagger i program.cs

## Tilføj Swagger til service
```
  builder.Services.AddEndpointsApiExplorer();
  builder.Services.AddSwaggerGen();
```
## Tilføj Swagger til udviklingsmiljø til app
```
  if (app.Environment.IsDevelopment())
    {
      app.UseSwagger();
      app.UseSwaggerUI();
    }
```

## Tilføj Swagger/OpenAPI support til API dokumentation og test via Swagger UI, så man kan se kommentarer fra api.
```
builder.Services.AddEndpointsApiExplorer();  
builder.Services.AddSwaggerGen(s =>   
{  
  s.SwaggerDoc("v1", new Microsoft.OpenApi.Models.OpenApiInfo  
  {   
      Title = "Semikolon API",   
      Version = "v1",   
      Description = "API for Semikolon application"  
  });   
 var xmlFile = $"{typeof(Program).Assembly.GetName().Name}.xml";   
 var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);   
 if (File.Exists(xmlPath))   
    {   
         s.IncludeXmlComments(xmlPath);   
    }   
 var xmlDomainFile = "Domain.xml";   
 var xmlDomainPath = Path.Combine(AppContext.BaseDirectory, xmlDomainFile);   
 if (File.Exists(xmlDomainPath))   
   {   
     s.IncludeXmlComments(xmlDomainPath);   
   }   
 var xmlApplicationFile = "Domain.xml";   
 var xmlApplicationPath = Path.Combine(AppContext.BaseDirectory, xmlApplicationFile);   
 if (File.Exists(xmlApplicationPath))   
  {    
     s.IncludeXmlComments(xmlApplicationPath);    
  }    
    });
```
