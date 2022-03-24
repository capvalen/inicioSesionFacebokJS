# Inicio Sesión en Facebok con JS
Ejemplo completo de Inicio de sesión con facebook con el API SDK de Javascript 13.0

Sugerencia para crear el aplicativo:
- Tu aplicativo debe estar creado sin **ninguna plantilla previa**
- El aplicativo debe estar en estado: **Publicada**
- No te olvides agregar en "Inicio de sesión con Facebook" > Configuración:
    - Poner como **SI**: Iniciar sesión con el SDK de JavaScript
    - Agregar tu dominio en: Dominios admitidos para el SDK de JavaScript
    
```
<!DOCTYPE html>
<html>
<head>
<title>Ejemplo de Login con Facebook y JavaScript </title>
<meta charset="UTF-8">
</head>
<body>
    <em>Sugerencia para crear el aplicativo:
    <ul>
        <li>Tu aplicativo debe estar creado sin ninguna plantilla previa</li>
        <li>El aplicativo debe estar en estado: Publicada</li>
        <li>No te olvides agregar en "Inicio de sesión con Facebook" > Configuración:
            <ul><li>Poner como <b>SI</b>: Iniciar sesión con el SDK de JavaScript</li><li>Agregar tu dominio en: Dominios admitidos para el SDK de JavaScript</li></ul
        </li>
    </ul></em>
    
<script>

  function cambioEstadoRespuesta(response) {
    console.log('Cambio de estado');
    console.log(response);                   // The current login status of the person.
    if (response.status === 'connected') {   // Logged into your webpage and Facebook.
      completarAcceso();  
    } else {                                 // Not logged into your webpage or we are unable to tell.
      document.getElementById('status').innerHTML = 'Loguéese primero ' +
        ' para entrar a esta web.';
    }
  }


  function verificarEstado() {               // Called when a person is finished with the Login Button.
    FB.getLoginStatus(function(response) {   // See the onlogin handler
      cambioEstadoRespuesta(response);
    });
  }


  window.fbAsyncInit = function() {
    FB.init({
      appId      : '4902880633140512',
      cookie     : true,                     // Enable cookies to allow the server to access the session.
      xfbml      : true,                     // Parse social plugins on this webpage.
      version    : 'v13.0'           // Use this Graph API version for this call.
    });


    FB.getLoginStatus(function(response) {   // Called after the JS SDK has been initialized.
      cambioEstadoRespuesta(response);        // Returns the login status.
    });
  };
 
  function completarAcceso() {                      // Testing Graph API after login.  See cambioEstadoRespuesta() for when this call is made.
    console.log('Bienvenido! Obteniendo tu información.... ');
    FB.api('/me', 'GET', {"fields":"id,name,email"}, function(response) {
      console.log('Ingreso completado como: ' + response.name);
      console.log('Su correo es: ' + response.email);
      
      document.getElementById('status').innerHTML =
        'Gracias por loguearte: ' + response.name + '!';
    });
  }
function salir(){
    FB.logout(function(response) {
  // user is now logged out
  console.log('salio');
  cambioEstadoRespuesta(response);
});
}
function iniciar(){
    FB.login(function(response) {
      
      if (response.status === 'connected') {   // Logged into your webpage and Facebook.
      completarAcceso();
      cambioEstadoRespuesta(response);
    } else {                                 // Not logged into your webpage or we are unable to tell.
      document.getElementById('status').innerHTML = 'Loguéese primero ' +
        ' para entrar a esta web.';
    }
    
}, {scope: 'public_profile,email'});
}
</script>


<!-- The JS SDK Login Button -->

<fb:login-button scope="public_profile,email" onlogin="verificarEstado();">
</fb:login-button>

<div id="status">
</div>
<button onclick="iniciar()"> Iniciar</button>
<button onclick="salir()"> Salir</button>

<!-- Load the JS SDK asynchronously -->
<script async defer crossorigin="anonymous" src="https://connect.facebook.net/en_US/sdk.js"></script>
</body>
</html>
```
