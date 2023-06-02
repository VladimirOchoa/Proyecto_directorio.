
# Main para el directorio.
import 'dart:io';
import 'lib/persona.dart';
import 'lib/alumno.dart';
import 'lib/profesor.dart';

class Directorio {
  List<Persona> personas = [];
  int contadorProfesores = 0;
  int contadorAlumnos = 0;

  void crearRegistro() {
    stdout.write('Ingrese el nombre: ');
    String? nombre = stdin.readLineSync();
    stdout.write('Ingrese el apellido paterno: ');
    String? apellidoPaterno = stdin.readLineSync();
    stdout.write('Ingrese el apellido materno: ');
    String? apellidoMaterno = stdin.readLineSync();
    stdout.write('Ingrese el correo electrónico universitario: ');
    String? correo;
    while (true) {
      correo = stdin.readLineSync();
      if (correo!.endsWith('@ucol.mx')) {
        break;
      } else {
        print('El correo electrónico debe ser tu correo universitario para poder registrarte.');
        print('Inténtelo nuevamente: ');
      }

    }
    


    String id;
    String tipo = seleccionarTipo();

    if (tipo == 'profesor') {
      contadorProfesores++;
      id = generarIDprofesores();
      Profesor profesor = Profesor(id, nombre!, apellidoPaterno!, apellidoMaterno!, correo);
      personas.add(profesor);
    } else {
      contadorAlumnos++;
      id = generarIDalumnos();
      Alumno alumno = Alumno(id, nombre!, apellidoPaterno!, apellidoMaterno!, correo);
      personas.add(alumno);
    }

    print('Registro creado exitosamente.');

    Persona persona = personas.last;
    print('ID: ${persona.id}');
    print('Nombre: ${persona.nombre}');
    print('Apellido paterno: ${persona.apellidoPaterno}');
    print('Apellido materno: ${persona.apellidoMaterno}');
    print('Tipo: ${persona is Profesor ? 'Profesor' : 'Alumno'}');
    print('Correo electrónico: ${persona.correo}');
  }

  String generarIDprofesores() {
    return 'p${contadorProfesores.toString().padLeft(2, '0')}';
  }

  String generarIDalumnos() {
    return 'a${contadorAlumnos.toString().padLeft(2, '0')}';
  }

  String seleccionarTipo() {
    while (true) {
      stdout.write('Seleccione el tipo: 1. Profesor, 2. Alumno: ');
      String? opcion = stdin.readLineSync();
      if (opcion == '1') {
        return 'profesor';
      } else if (opcion == '2') {
        return 'alumno';
      } else {
        print('No ha seleccionado una opción válida. Inténtelo nuevamente.');
      }
    }
  }

  void leerRegistros() {
    if (personas.isEmpty) {
      print('El directorio está vacío.');
    } else {
      print('----- Menú de Lectura -----');
      print('1. Leer todos los registros');
      print('2. Leer registros de profesores');
      print('3. Leer registros de alumnos');
      print('4. Buscar registro por ID');
      print('5. Regresar al menú principal');
      stdout.write('Ingrese una opción: ');
      String? opcion = stdin.readLineSync();

      switch (opcion) {
        case '1':
          print('--- Todos los registros ---');
          mostrarRegistros(personas);
          break;
        case '2':
          print('--- Registros de profesores ---');
          List<Persona> profesores = personas.where((persona) => persona is Profesor).toList();
          mostrarRegistros(profesores);
          break;
        case '3':
          print('--- Registros de alumnos ---');
          List<Persona> alumnos = personas.where((persona) => persona is Alumno).toList();
          mostrarRegistros(alumnos);
          break;
        case '4':
         print('Ingrese el ID: ');
          String? id = stdin.readLineSync();
          if (id!.startsWith('p')) {	
            // ignore: null_check_always_fails
            Profesor? profesor = personas.firstWhere((persona) => persona.id == id, orElse: () => null!) as Profesor?;
            if (profesor == null) {
              print('No se encontró el registro.');
            } else {
              print('--- Registro encontrado ---');
              print('ID: ${profesor.id}');
              print('Nombre: ${profesor.nombre}');
              print('Apellido paterno: ${profesor.apellidoPaterno}');
              print('Apellido materno: ${profesor.apellidoMaterno}');
              print('Correo electrónico: ${profesor.correo}');
            }
          } else if (id.startsWith('a')) {
            // ignore: null_check_always_fails
            Alumno? alumno = personas.firstWhere((persona) => persona.id == id, orElse: () => null!) as Alumno?;
            if (alumno == null) {
              print('No se encontró el registro.');
            } else {
              print('--- Registro encontrado ---');
              print('ID: ${alumno.id}');
              print('Nombre: ${alumno.nombre}');
              print('Apellido paterno: ${alumno.apellidoPaterno}');
              print('Apellido materno: ${alumno.apellidoMaterno}');
              print('Correo electrónico: ${alumno.correo}');
            }
          } else {
            print('No se encontró el registro.');
          }
          break;
          case '5':
          print('Regresando al menú principal...');
          break;
        default:
          print('Opción inválida. Inténtelo nuevamente.');
          break;
      }
    }
  }

  void mostrarRegistros(List<Persona> registros) {
    if (registros.isEmpty) {
      print('No se encontraron registros.');
    } else {
      for (Persona persona in registros) {
        print('ID: ${persona.id}');
        print('Nombre: ${persona.nombre}');
        print('Apellido paterno: ${persona.apellidoPaterno}');
        print('Apellido materno: ${persona.apellidoMaterno}');
        print('Tipo: ${persona is Profesor ? 'Profesor' : 'Alumno'}');
        print('Correo electrónico: ${persona.correo}');
        print('-------------------------');
      }
    }
  }

  void actualizarRegistro() {
    if (personas.isEmpty) {
      print('El directorio está vacío.');
    } else {
      stdout.write('Ingrese el ID del registro a actualizar: ');
      String? id = stdin.readLineSync();
      // ignore: null_check_always_fails
      Persona? persona = personas.firstWhere((persona) => persona.id == id, orElse: () => null!);

      // ignore: unnecessary_null_comparison
      if (persona != null) {
        stdout.write('Ingrese el nuevo nombre: ');
        String? nombre = stdin.readLineSync();
        persona.nombre = nombre!;
        stdout.write('Ingrese el nuevo apellido paterno: ');
        String? apellidoPaterno = stdin.readLineSync();
        persona.apellidoPaterno = apellidoPaterno!;
        stdout.write('Ingrese el nuevo apellido materno: ');
        String? apellidoMaterno = stdin.readLineSync();
        persona.apellidoMaterno = apellidoMaterno!;
        stdout.write('Ingrese el nuevo correo electrónico: ');
        String? correo = stdin.readLineSync();
        persona.correo = correo!;
        print('Registro actualizado exitosamente.');
      } else {
        print('No se encontró ningún registro con ese ID.');
      }
    }
  }

  void eliminarRegistro() {
    if (personas.isEmpty) {
      print('El directorio está vacío.');
    } else {
      stdout.write('Ingrese el ID del registro a eliminar: ');
      String? id = stdin.readLineSync();
      while (id!.startsWith('p') || id.startsWith('a')) {
        // ignore: null_check_always_fails
        Persona? persona = personas.firstWhere((persona) => persona.id == id, orElse: () => null!);
        // ignore: unnecessary_null_comparison
        if (persona == null) {
          print('No se encontró ningún registro con ese ID.');
          break;
        } else {
          personas.remove(persona);
          print('Registro eliminado exitosamente.');
          break;
        }
     }
    }
  }
}

void main() {
  Directorio directorio = Directorio();

  while (true) {
    print('----- Menú -----');
    print('1. Crear registro');
    print('2. Leer registros');
    print('3. Actualizar registro');
    print('4. Eliminar registro');
    print('5. Salir');
    stdout.write('Ingrese una opción: ');
    String? opcion = stdin.readLineSync();

    switch (opcion) {
      case '1':
        directorio.crearRegistro();
        break;
      case '2':
        directorio.leerRegistros();
        break;
      case '3':
        directorio.actualizarRegistro();
        break;
      case '4':
        directorio.eliminarRegistro();
        break;
      case '5':
        return;
      default:
        print('No ha seleccionado una opción válida. Inténtelo nuevamente.');
        break;
    }

    print('');
  }
}
