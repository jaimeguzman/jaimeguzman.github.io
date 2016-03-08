
# Envio de correo a traves de gmail con redmine


[Redmine](http://www.redmine.org/) es un gestor de proyectos el cual te avisa mediante correo electrónico  de las nuevas tareas o cambios en las mismas. Aquí es donde entra GMail en acción ;-)

Dejo la configuración que me ha funcionado por si le puede servir a alguien o por si la puedo reusar yo mismo.

```
production:
  delivery_method: :smtp
  smtp_settings:
    enable_starttls_auto: true
    tls: true
    address: "smtp.googlemail.com"
    port: 587
    domain: "smtp.googlemail.com" # 'your.domain.com' for GoogleApps
    authentication: :plain
    user_name: "micorreo@gmail.com"
    password: "mipass!"
```

[redmineblog.com](http://redmineblog.com/articles/setup-redmine-to-send-email-using-gmail/)
