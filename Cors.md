# CORS
*Væsentligt for kommunikation mellen app og api.  Cross-Origin Resource Sharing*

```
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAngularDev",
        builder =>
        {
            builder.WithOrigins("http://localhost:4200")
                   .AllowAnyHeader()
                   .AllowAnyMethod();
        });
});

app.UseCors("AllowAngularDev");
```


## Fejlbeskeden med F12 i console ser sådan ud:
```
Access to XMLHttpRequest at 'https://localhost:7279/api/auth/login' from origin 'http://localhost:4200'
has been blocked by CORS policy:
Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' 
header is present on the requested resource.
```
samt oplysninger om det specifikke kald:
```
login.ts:60 
 POST https://localhost:7279/api/auth/login net::ERR_FAILED
handleLogin	@	login.ts:60
LoginComponent_Template_form_submit_0_listener	@	login.ts:13

```
