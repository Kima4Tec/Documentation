
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

            // Add Swagger/OpenAPI support (for API documentation and testing via Swagger UI)
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

## Tilføj Swagger med authentication

```
            builder.Services.AddEndpointsApiExplorer();
            builder.Services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("v1", new OpenApiInfo { Title = "FilmFrame API", Version = "v1" });

                // Tilføj JWT Bearer support
                c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
                {
                    Name = "Authorization",
                    Type = SecuritySchemeType.ApiKey,
                    Scheme = "Bearer",
                    BearerFormat = "JWT",
                    In = ParameterLocation.Header,
                    Description = "Indtast 'Bearer {token}' i headeren. Eksempel: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9..."
                });

                c.AddSecurityRequirement(new OpenApiSecurityRequirement
```
eller denne, som også virker:
```
            builder.Services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("v1", new OpenApiInfo
                {
                    Title = "Login API",
                    Version = "v1"
                });

                // Fortæl Swagger hvordan JWT ser ud
                c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
                {
                    Name = "Authorization",
                    Type = SecuritySchemeType.Http,
                    Scheme = "Bearer",
                    BearerFormat = "JWT",
                    In = ParameterLocation.Header,
                    Description = "Indtast JWT token sådan her: Bearer {token}"
                });

                // Gør Authorize global for endpoints
                c.AddSecurityRequirement(new OpenApiSecurityRequirement
                {
                    {
                        new OpenApiSecurityScheme
                        {
                            Reference = new OpenApiReference
                            {
                                Type = ReferenceType.SecurityScheme,
                                Id = "Bearer"
                            }
                        },
                        new string[] {}
                    }
                });
            });
```
