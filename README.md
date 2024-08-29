package iti.data;

import iti.modelo.Estudiante;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import static iti.connect.Connect.getConnection;

// DAO - Data Access Object
public class EstudianteDAO {
    public List<Estudiante> listarEstudiantes(){
        List<Estudiante> estudiantes = new ArrayList<>();

        // Trabajo con clase de conexion a BD
        PreparedStatement ps;
        ResultSet rs;
        Connection conect = getConnection();
        String sql = "SELECT * FROM estudiante ORDER BY id_estudiante;";
        try{
            ps = conect.prepareStatement(sql);
            rs = ps.executeQuery();
            while (rs.next()){
                var estudiante = new Estudiante();
                estudiante.setIdEstudiante(rs.getInt("id_estudiante"));
                estudiante.setCedula(rs.getString("cedula"));
                estudiante.setNombre(rs.getString("nombre"));
                estudiante.setApellido(rs.getString("apellido"));
                estudiante.setTelefono(rs.getString("telefono"));
                estudiante.setEmail(rs.getString("email"));
                estudiantes.add(estudiante);
            }
        }
        catch (SQLException e){
            System.out.println("Ocurrio un error al listar los estudiantes: " + e.getMessage());
        }
        finally {
            try{
                conect.close();
            }
            catch (Exception e){
                System.out.println("Error al cerrar conexion " + e.getMessage());
            }
        }
        return estudiantes;
    }

    public boolean findById(Estudiante estudiante) {
        PreparedStatement ps;
        ResultSet rs;
        Connection conect = getConnection();
        String sql = "SELECT * FROM estudiante WHERE id_estudiante = ?;";
        try{
            ps = conect.prepareStatement(sql);
            ps.setInt(1, estudiante.getIdEstudiante());
            rs = ps.executeQuery();
            while (rs.next()){
                estudiante.setCedula(rs.getString("cedula"));
                estudiante.setNombre(rs.getString("nombre"));
                estudiante.setApellido(rs.getString("apellido"));
                estudiante.setTelefono(rs.getString("telefono"));
                estudiante.setEmail(rs.getString("email"));
                return true;
            }
        }
        catch (SQLException e){
            System.out.println("Ocurrio un error al buscar estudiante: " + e.getMessage());
        }
        finally {
            try{
                conect.close();
            }
            catch (Exception e){
                System.out.println("Error al cerrar conexion " + e.getMessage());
            }
        }
        return false;
    }

    public boolean agregarEstudiante(Estudiante estudiante){
        PreparedStatement ps;
        Connection conect = getConnection();
        String sql = "INSERT INTO estudiante(cedula, nombre, apellido, telefono, email) " +
                "VALUES(?, ?, ?, ?, ?);";
        try{
            ps = conect.prepareStatement(sql);
            ps.setString(1, estudiante.getCedula());
            ps.setString(2, estudiante.getNombre());
            ps.setString(3, estudiante.getApellido());
            ps.setString(4, estudiante.getTelefono());
            ps.setString(5, estudiante.getEmail());
            ps.execute();
            return true;
        }
        catch (SQLException e){
            System.out.println("Ocurrio un error al agregar estudiante: " + e.getMessage());
        }
        finally {
            try{
                conect.close();
            }
            catch (Exception e){
                System.out.println("Error al cerrar conexion " + e.getMessage());
            }
        }
        return false;
    }

    public boolean actualizarEstudiante(Estudiante estudiante){
        PreparedStatement ps;
        Connection conect = getConnection();
        String sql = "UPDATE estudiante SET cedula=?, nombre=?, apellido=?, telefono=?, email=? " +
                "WHERE id_estudiante=?;";
        try{
            ps = conect.prepareStatement(sql);
            ps.setString(1, estudiante.getCedula());
            ps.setString(2, estudiante.getNombre());
            ps.setString(3, estudiante.getApellido());
            ps.setString(4, estudiante.getTelefono());
            ps.setString(5, estudiante.getEmail());
            ps.setInt(6, estudiante.getIdEstudiante());
            ps.execute();
            return true;
        }
        catch (SQLException e){
            System.out.println("Ocurrio un error al actualizar estudiante: " + e.getMessage());
        }
        finally {
            try{
                conect.close();
            }
            catch (Exception e){
                System.out.println("Error al cerrar conexion " + e.getMessage());
            }
        }
        return false;
    }

    public boolean eliminarEstudiante(Estudiante estudiante){
        PreparedStatement ps;
        Connection conect = getConnection();
        String sql = "DELETE FROM estudiante WHERE id_estudiante = ?;";
        try{
            ps = conect.prepareStatement(sql);
            ps.setInt(1, estudiante.getIdEstudiante());
            ps.execute();
            return true;
        }
        catch (SQLException e){
            System.out.println("Ocurrio un error al eliminar estudiante: " + e.getMessage());
        }
        finally {
            try{
                conect.close();
            }
            catch (Exception e){
                System.out.println("Error al cerrar conexion " + e.getMessage());
            }
        }
        return false;
    }
    public Estudiante buscarEstudiantePorCedula(String cedula) {
        PreparedStatement ps;
        ResultSet rs;
        Connection conect = getConnection();
        String sql = "SELECT * FROM estudiante WHERE cedula = ?;";
        Estudiante estudiante = null;

        try {
            ps = conect.prepareStatement(sql);
            ps.setString(1, cedula);
            rs = ps.executeQuery();

            if (rs.next()) {
                estudiante = new Estudiante();
                estudiante.setIdEstudiante(rs.getInt("id_estudiante"));
                estudiante.setCedula(rs.getString("cedula"));
                estudiante.setNombre(rs.getString("nombre"));
                estudiante.setApellido(rs.getString("apellido"));
                estudiante.setTelefono(rs.getString("telefono"));
                estudiante.setEmail(rs.getString("email"));
            }
        } catch (SQLException e) {
            System.out.println("Ocurrió un error al buscar estudiante por cédula: " + e.getMessage());
        } finally {
            try {
                conect.close();
            } catch (Exception e) {
                System.out.println("Error al cerrar conexión: " + e.getMessage());
            }
        }

        return estudiante;
    }
    public boolean eliminarEstudiantePorCedula(String cedula) {
        PreparedStatement ps;
        Connection conect = getConnection();
        String sql = "DELETE FROM estudiante WHERE cedula = ?;";

        try {
            ps = conect.prepareStatement(sql);
            ps.setString(1, cedula);
            ps.execute();
            return true;
        } catch (SQLException e) {
            System.out.println("Ocurrió un error al eliminar estudiante por cédula: " + e.getMessage());
        } finally {
            try {
                conect.close();
            } catch (Exception e) {
                System.out.println("Error al cerrar conexión: " + e.getMessage());
            }
        }

        return false;
    }


    public static void main(String[] args){
        var estudianteDAO = new EstudianteDAO();
        // Buscar por ID
        /*var estudiante1 = new Estudiante(5);
        var existeEstudiabte = estudianteDAO.findById(estudiante1);
        if(existeEstudiabte)
            System.out.println("Estudiante encontrado: " + estudiante1);
        else
            System.out.println("No se encontro estudiante con ID : " + estudiante1.getIdEstudiante());*/

        // Agregar Estudiante nuevo
        /*var estudiante_new = new Estudiante("1717171717", "Jimena", "Suarez", "0912345678", "jimena@mail.com");
        var agregaEstudiante = estudianteDAO.agregarEstudiante(estudiante_new);

        if(agregaEstudiante)
            System.out.println("Estudiante agregado exitosamente: " + estudiante_new);
        else
            System.out.println("No se pudo agregar estudiante : " + estudiante_new);*/

        // Actualizar estudiante
        /*var edit_estudiante = new Estudiante(3, "0985274118", "Marianela",
                "Lopez", "0987654321", "marianela@mail.com");
        var actualizado = estudianteDAO.actualizarEstudiante(edit_estudiante);
        if(actualizado)
            System.out.println("Estudiante actualizado exitosamente: " + edit_estudiante);
        else
            System.out.println("No se pudo actualizar estudiante : " + edit_estudiante);*/

        // Eliminar estudiante
        var delete_estudiante = new Estudiante(4);
        var eliminado = estudianteDAO.eliminarEstudiante(delete_estudiante);
        if(eliminado)
            System.out.println("Estudiante eliminado exitosamente: " + delete_estudiante);
        else
            System.out.println("No se pudo eliminar estudiante : " + delete_estudiante);

        // Listar estudiantes
        System.out.println();
        System.out.println("Lista de estudiantes: ");
        List<Estudiante> estudiantes = estudianteDAO.listarEstudiantes();
        estudiantes.forEach(System.out::println);
    }
}
package iti.vista;

import iti.data.EstudianteDAO;
import iti.modelo.Estudiante;

import java.util.List;
import java.util.Scanner;

public class EstudiantesApp {
    public static void main(String[] args){
        Scanner consola = new Scanner(System.in);
        var salir = false;
        var estudianteDAO = new EstudianteDAO();

        // MUESTRA MENU
        while (!salir){
            try{
                muestramenu();
                salir = ejecutarOpciones(consola, estudianteDAO);
            }
            catch (Exception e){
                System.out.println();
                System.out.println("Error en ejecución: " + e.getMessage());
                System.out.println();
            }
        }

    }

    private static void muestramenu(){
        System.out.println();
        System.out.println("""
                ***** SISTEMA DE ESTUDIANTES *****
                1. Listar estudiantes
                2. Buscar estudiante
                3. Agregar estudiante
                4. Actualizar estudiante
                5. Eliminar estudiante
                6. Salir
                """);
        System.out.print("Por favor, ingrese una opción: ");
        System.out.println();
    }

    private static boolean ejecutarOpciones(Scanner consola, EstudianteDAO estudianteDAO){
        var opcion = Integer.parseInt(consola.nextLine());
        System.out.println();
        boolean salir = false;
        // Validar opciones ingresadas
        switch (opcion){
            case 1 -> { // Listar estudiantes
                System.out.println();
                System.out.println(".:: Lista de estudiantes ::.");
                var estudiantes = estudianteDAO.listarEstudiantes();
                estudiantes.forEach(System.out::println);
            }
            case 2 -> { // Buscar por ID
                System.out.println("Ingrese ID de estudiante a buscar: ");
                var idEstudiante = Integer.parseInt(consola.nextLine());
                var estudiante = new Estudiante(idEstudiante);
                var existeEstudiante = estudianteDAO.findById(estudiante);
                if(existeEstudiante)
                    System.out.println("Estudiante encontrado: " + estudiante);
                else
                    System.out.println("No se encontro estudiante con ID : " + estudiante.getIdEstudiante());
            }
            case 3 -> { // Agregar estudiante
                System.out.println(".:: Agregar Estudiante ::.");
                System.out.print("Cedula: ");
                var cedula = consola.nextLine();
                System.out.print("Nombre: ");
                var nombre = consola.nextLine();
                System.out.print("Apellido: ");
                var apellido = consola.nextLine();
                System.out.print("Teléfono: ");
                var telefono = consola.nextLine();
                System.out.print("Email: ");
                var email = consola.nextLine();

                var nuevoEstudiante = new Estudiante(cedula, nombre, apellido, telefono, email);
                var agregaEstudiante = estudianteDAO.agregarEstudiante(nuevoEstudiante);

                if(agregaEstudiante)
                    System.out.println("Estudiante agregado exitosamente: " + nuevoEstudiante);
                else
                    System.out.println("No se pudo agregar estudiante : " + nuevoEstudiante);

            }

            case 4 -> { // Actualizar estudiante
                System.out.println(".:: Actualizar Estudiante ::.");
                System.out.println("Ingrese ID de estudiante a actualizar: ");
                var idEstudiante = Integer.parseInt(consola.nextLine());
                System.out.print("Cedula: ");
                var cedula = consola.nextLine();
                System.out.print("Nombre: ");
                var nombre = consola.nextLine();
                System.out.print("Apellido: ");
                var apellido = consola.nextLine();
                System.out.print("Teléfono: ");
                var telefono = consola.nextLine();
                System.out.print("Email: ");
                var email = consola.nextLine();

                var editaEstudiante = new Estudiante(idEstudiante, cedula, nombre, apellido, telefono, email);
                var actualizado = estudianteDAO.actualizarEstudiante(editaEstudiante);
                if(actualizado)
                    System.out.println("Estudiante actualizado exitosamente: " + editaEstudiante);
                else
                    System.out.println("No se pudo actualizar estudiante : " + editaEstudiante);
            }
            case 5 -> { // Eliminar estudiante
                System.out.println(".:: Eliminar Estudiante ::.");
                System.out.println("Ingrese ID de estudiante a eliminar: ");
                var idEstudiante = Integer.parseInt(consola.nextLine());
                var eliminaEstudiante = new Estudiante(idEstudiante);
                var eliminado = estudianteDAO.eliminarEstudiante(eliminaEstudiante);
                if(eliminado)
                    System.out.println("Estudiante eliminado exitosamente: " + eliminaEstudiante);
                else
                    System.out.println("No se pudo eliminar estudiante : " + eliminaEstudiante);
            }
            case 6 -> { // Buscar estudiante por cédula
                System.out.println("Ingrese la cédula del estudiante a buscar: ");
                var cedula = consola.nextLine();
                var estudiante = estudianteDAO.buscarEstudiantePorCedula(cedula);
                if(estudiante != null)
                    System.out.println("Estudiante encontrado: " + estudiante);
                else
                    System.out.println("No se encontró estudiante con cédula: " + cedula);
            }
            case 7 -> { // Eliminar estudiante por cédula
                System.out.println("Ingrese la cédula del estudiante a eliminar: ");
                var cedula = consola.nextLine();
                var eliminado = estudianteDAO.eliminarEstudiantePorCedula(cedula);
                if(eliminado)
                    System.out.println("Estudiante eliminado exitosamente.");
                else
                    System.out.println("No se pudo eliminar estudiante con cédula: " + cedula);
            }
            case 8 -> { // Salir de la app
                System.out.println("Gracias, hasta pronto!!!");
                salir = true;
            }
            default -> System.out.println("Opcion ingresada no existe, por favor ingrese nuevamente.");
        }
        return salir;
    }

}
