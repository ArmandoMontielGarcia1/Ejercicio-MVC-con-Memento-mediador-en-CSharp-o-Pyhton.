# Ejercicio-MVC-con-Memento-mediador-en-CSharp-o-Pyhton.
# Nombre: Motiel Garcia Armando #20210602
# Descripción de la Actividad
Objetivo: Implementar los patrones de diseño Modelo-Vista-Controlador (MVC) y Mediador para desarrollar una aplicación de dibujo colaborativo en línea, que permita a múltiples usuarios interactuar y modificar un lienzo compartido en tiempo real.
# Codigo:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    public class ModeloLienzo
    {
        private List<string> elementos = new List<string>(); // Lista de elementos gráficos

        public void AgregarElemento(string elemento)
        {
            elementos.Add(elemento);
            Console.WriteLine("Se agregó un elemento: " + elemento);
        }

        public void EliminarElemento(string elemento)
        {
            elementos.Remove(elemento);
            Console.WriteLine("Se eliminó un elemento: " + elemento);
        }

        public void ModificarElemento(string elementoAntiguo, string elementoNuevo)
        {
            int index = elementos.IndexOf(elementoAntiguo);
            if (index != -1)
            {
                elementos[index] = elementoNuevo;
                Console.WriteLine("Se modificó un elemento de " + elementoAntiguo + " a " + elementoNuevo);
            }
            else
            {
                Console.WriteLine("El elemento a modificar no existe.");
            }
        }

        // Método para obtener los elementos del lienzo
        public List<string> ObtenerElementos()
        {
            return elementos;
        }
    }

    // Clase Vista
    public class VistaLienzo
    {
        public void MostrarLienzo(ModeloLienzo modelo)
        {
            Console.WriteLine("Estado actual del lienzo:");
            foreach (var elemento in modelo.ObtenerElementos())
            {
                Console.WriteLine(elemento);
            }
        }
    }

    // Clase Controlador
    public class ControladorLienzo
    {
        private ModeloLienzo modelo;
        private VistaLienzo vista;

        public ControladorLienzo(ModeloLienzo modelo, VistaLienzo vista)
        {
            this.modelo = modelo;
            this.vista = vista;
        }

        public void AgregarElemento(string elemento)
        {
            modelo.AgregarElemento(elemento);
            vista.MostrarLienzo(modelo);
        }

        public void EliminarElemento(string elemento)
        {
            modelo.EliminarElemento(elemento);
            vista.MostrarLienzo(modelo);
        }

        public void ModificarElemento(string elementoAntiguo, string elementoNuevo)
        {
            modelo.ModificarElemento(elementoAntiguo, elementoNuevo);
            vista.MostrarLienzo(modelo);
        }
    }

    // Clase Mediador
    public class MediadorSesion
    {
        private List<ControladorLienzo> controladores = new List<ControladorLienzo>();

        public void AgregarControlador(ControladorLienzo controlador)
        {
            controladores.Add(controlador);
        }

        public void SincronizarElemento(string elemento)
        {
            foreach (var controlador in controladores)
            {
                controlador.AgregarElemento(elemento); // Sincronizar agregando el elemento en todos los controladores
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Crear instancias
            ModeloLienzo modelo = new ModeloLienzo();
            VistaLienzo vista = new VistaLienzo();
            ControladorLienzo controlador = new ControladorLienzo(modelo, vista);
            MediadorSesion mediador = new MediadorSesion();

            // Agregar controlador al mediador
            mediador.AgregarControlador(controlador);

            // Simular acciones de usuario
            controlador.AgregarElemento("Círculo");
            controlador.AgregarElemento("Cuadrado");
            controlador.EliminarElemento("Círculo");
            controlador.ModificarElemento("Cuadrado", "Triángulo");

            // Sincronizar elementos con el mediador
            mediador.SincronizarElemento("Rectángulo");

            Console.ReadLine();
        }
    }
}

![image](https://github.com/ArmandoMontielGarcia1/Ejercicio-MVC-con-Memento-mediador-en-CSharp-o-Pyhton./assets/144396511/2e07f000-3a34-4fd3-bbc2-a73ee385da7a)
