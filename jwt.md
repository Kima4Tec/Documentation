# JWT i API

## Tilføj JWT til program.cs

```
            builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
           .AddJwtBearer(options =>
           {
               options.TokenValidationParameters = new TokenValidationParameters
               {
                   ValidateIssuer = true,
                   ValidateAudience = true,
                   ValidateLifetime = true,
                   ValidateIssuerSigningKey = true,
                   ValidIssuer = builder.Configuration["Jwt:Issuer"],
                   ValidAudience = builder.Configuration["Jwt:Audience"],
                   IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Secret"] ?? throw new InvalidOperationException("JWT Secret is not configured")))
               };
           });
```


## Tilføj Token i controller
```
        //Opretter JWT token baseret på brugerens oplysninger og roller
        private string CreateToken(User user)
        {
            var claims = new List<Claim>
            {
                new Claim(ClaimTypes.Name, user.LoginName),
                new Claim(ClaimTypes.Role, user.Role?.ToString() ?? "Bruger")
            };

            var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(
                _configuration.GetValue<string>("Jwt:Secret")!));

            var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha512);

            var tokenDescriptor = new JwtSecurityToken(
                issuer: _configuration.GetValue<string>("Jwt:Issuer"),
                audience: _configuration.GetValue<string>("Jwt:Audience"),
                claims: claims,
                expires: DateTime.UtcNow.AddDays(1),
                signingCredentials: creds
            );

            return new JwtSecurityTokenHandler().WriteToken(tokenDescriptor);
        }
```

## Få token ved login
```
        //Login med brugernavn og password - får et JWT token tilbage ved succesfuldt login
        [HttpPost("login")]
        public async Task<IActionResult> Login([FromBody] LoginRequest request)
        {
            var user = await _userService.GetUserByUsernameAsync(request.UserName);
            if (user != null && !user.IsFirstTimeLoggedIn && user.isActive &&
                _userService.VerifyPassword(request.Password, user.Password))
            {
                string token = CreateToken(user);
                return Ok(new { token });
            }

            return Unauthorized(new { message = "Ugyldigt login" });
        }

```


## Appsettings.json
```
  "Jwt": {
    "Token": "MySuperSecureAndRandomnKeyThatsLooksJustAwesomeAndNeedsToBeVeryVeryLong!!!llloneeleven",
    "Issuer": "MyAwesomeApp",
    "Audience": "MyAweSomeAudience",
    "Secret": "8d8b7e72d97b4a5b8f8f9b64512fdbd0987028b7ecf34fc4f0c0c0f41b8d0b59"
  },
```
