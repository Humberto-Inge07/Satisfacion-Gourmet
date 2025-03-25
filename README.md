# Satisfacion-Gourmet
ENCUESTA DE SATISFACION DE LOS CLIENTES DE UN RESTAURANTE.
Lista de Herramientas
Base de datos en MySQL.  

Google Forms: Para crear el formulario.

Google Sheets: Para almacenar las respuestas.

Google Apps Script: Para conectar Google Sheets con MySQL .

MySQL: Para almacenar los datos finales.

  PRIMERO: creamos un formulario usando google forms     Este link nos muestra el formulario //https://forms.gle/f4bzaACsUeSTqvyf7  -formulario-//

  En el formulario, vamos a la pestaña "Respuestas"

Hacemos clic en el icono de Google Sheets (hoja verde).  Este link nos muestra la hoja de calculo con los datos almcenados https://docs.google.com/spreadsheets/d/1Q6E84xDbic5GrShjXrPpzW6C1yRq-zVQ0222iU4H1hk/edit?resourcekey=&gid=229076045#gid=229076045  -hoja de calculo-


Eligimos "Crear una nueva hoja de cálculo" y asígnala un nombre como "Respuestas_Encuesta_Gourmet"

SEGUNDO: Creamos una base de datos en mysql con varias tablas, de las cuales una es de CLIENTES.

REATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,          -- Identificador único (clave primaria)
    nombre_completo VARCHAR(100) NOT NULL,     -- Nombre completo (máximo 100 caracteres)
    email VARCHAR(100) NOT NULL,               -- Correo electrónico
    telefono VARCHAR(20) NOT NULL,             -- Número de teléfono
    calificacion_servicio INT NOT NULL,        -- Calificación del servicio (1 a 5)
    recomendacion ENUM('Sí', 'No') NOT NULL,   -- Recomendación (Sí/No)
    fecha_respuesta TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- Fecha y hora de la respuesta
);
// En MySQL, el comando TIMESTAMP se utiliza para almacenar la fecha y hora, mientras que CURRENT_TIMESTAMP devuelve la fecha y hora actuales. 
TIMESTAMP 
Es un tipo de dato que almacena la fecha y hora.
Es útil para registrar el momento exacto en el que ocurrió un evento.
Es esencial para sistemas que necesitan realizar seguimientos detallados, como los sistemas de logs, temporizadores de eventos o cualquier aplicación que requiera auditoría de actividades.
CURRENT_TIMESTAMP
Es una función que devuelve la fecha y hora actuales. 
La fecha y hora se devuelven como "AAAA-MM-DD HH-MM-SS" (cadena) o como AAAAMMDDHHMMSS.uuuuuu (numérico). 
Se puede usar para definir el momento en que se agregó o actualizó una fila. 
Por defecto, se le asigna automáticamente la fecha y hora actual cuando se inserta o actualiza un registro. 
Para mayor precisión en los tiempos, se puede utilizar DATETIME(6) o TIMESTAMP(6) para almacenar tiempos con una precisión de hasta microsegundos. //

TERCERO: Google Apps Script: Para conectar Google Sheets con MySQL .

function enviarDatosMySQL() {
  // Configuración de la base de datos MySQL
  const url = 'jdbc:mysql://localhost:3306/nombre_de_tu_base_de_datos';
  const user = 'tu_usuario_mysql';
  const password = 'tu_contraseña_mysql';

  // Conectar a la base de datos
  const conn = Jdbc.getConnection(url, user, password);

  // Obtener la hoja de cálculo activa
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = sheet.getDataRange().getValues();

  // Insertar datos en MySQL
  for (let i = 1; i < data.length; i++) {
    const row = data[i];
    const nombre = row[1]; // Columna B
    const email = row[2]; // Columna C
    const telefono = row[3]; // Columna D
    const calificacion = row[4]; // Columna E
    const recomendacion = row[5]; // Columna F

    const query = `INSERT INTO respuestas (nombre_completo, email, telefono, calificacion_servicio, recomendacion)
                   VALUES ('${nombre}', '${email}', '${telefono}', ${calificacion}, '${recomendacion}')`;

    const stmt = conn.createStatement();
    stmt.executeUpdate(query);
  }

  // Cerrar la conexión
  conn.close();
}

YA CONECTADOS A LA BASE DE DATOS, USAMOS UN GENERADOR DE CODIGOS QR  //QR Code Generator | Create QR Codes// 
TERMINBADO ESTO EL QR SE LE PASARA A LOS CLIENTES DEL RESTAURANTE PARA QUE PROCEDAN A EVALUARNOS.






