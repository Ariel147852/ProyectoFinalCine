import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.Random;
import java.util.Scanner;

public class proyectoFinal {
    public static void main(String[] args) {
        // Se crea un objeto Scanner para leer datos desde la consola
        Scanner scanner = new Scanner(System.in);
        
        // Declaración de la matriz tridimensional para los asientos:
        // - Primer índice: representa la sala (3 salas en total)
        // - Segundo índice: representa la fila dentro de la sala (6 filas: A a F)
        // - Tercer índice: representa el número de asiento dentro de la fila (10 asientos por fila)
        char[][][] salas = new char[3][6][10];
        
        // Arreglos para llevar el conteo de boletos vendidos y de ingresos por cada sala:
        int[] boletosVendidos = new int[3];    // Guarda la cantidad de boletos vendidos por sala
        double[] ingresosPorSala = new double[3]; // Guarda el total de ingresos (dinero) por sala
        
        // Arreglo para contar la cantidad de cada tipo de snack vendido (5 tipos de snacks)
        int[] snacksVendidos = new int[5];
        
        // Inicialización de la matriz "salas":
        // Se recorre cada sala, cada fila y cada asiento en la fila para asignar 'O',
        // que indica que el asiento está libre.
        for (int s = 0; s < salas.length; s++) {                   // salas.length es 3, el número de salas.
            for (int f = 0; f < salas[s].length; f++) {              // salas[s].length es 6, número de filas en la sala.
                for (int a = 0; a < salas[s][f].length; a++) {       // salas[s][f].length es 10, número de asientos por fila.
                    salas[s][f][a] = 'O'; // 'O' indica que el asiento está disponible.
                }
            }
        }

        // Datos fijos para la cartelera del cine:
        // Arreglos que contienen la información de las películas y otros datos asociados.
        String[] peliculas = {"Spider-Man: Multiverso", "Frozen 3", "Oppenheimer"};
        String[] tipos = {"2D", "3D", "VIP"};
        double[] precios = {3.99, 4.50, 6.00};
        String[] horarios = {"21:00", "19:30", "17:00"};
        // Información sobre snacks y sus precios respectivos.
        String[] snacks = {"Papas + Refresco", "Palomitas + Jugo", "Nachos + Agua", "Galletas + Café", "Chocolatina + Té"};
        double[] snackPrecios = {3.50, 4.00, 3.00, 4.50, 3.25};

        // Variable de control para salir del menú principal.
        boolean salir = false;
        
        // Bucle principal del menú: se repite hasta que el usuario seleccione la opción de salir (opción 3).
        while (!salir) {
            // MENÚ PRINCIPAL: Se muestran las opciones disponibles.
            System.out.println("\n========== CodeCine ==========");
            System.out.println("1. Comprar entradas");
            System.out.println("2. ver reporte de ventas");
            System.out.println("3. Salir");
            System.out.print("Ingrese una opción: ");
            
            // Se lee la opción seleccionada por el usuario y se limpia el buffer de entrada.
            int opcion = scanner.nextInt();
            scanner.nextLine(); 
            
            // Se evalúa la opción seleccionada mediante una estructura switch.
            switch (opcion) {
                case 1:
                    // PROCESO DE COMPRA:
                    // Este bloque permite al usuario realizar una compra de entradas (y snacks) con la opción
                    // de repetir la compra sin volver al menú principal si así lo desea.
                    String otraCompra;  // Variable para controlar si se repite la compra.
                    do {
                        // Bienvenida al proceso de compra.
                        // Se omite la solicitud del nombre del cliente según el requerimiento.
                        System.out.println("\nBienvenido a CodeCine!");
                        
                        // Mostrar la cartelera con la información de las películas.
                        System.out.println("\nCartelera:");
                        for (int i = 0; i < peliculas.length; i++) {
                            // Se muestra la información de cada sala junto con la película, tipo, precio y horario.
                            System.out.println("Sala " + (i + 1) + " - " + tipos[i] 
                                + " - " + peliculas[i] + " | Precio: $" + precios[i]
                                + " | Horario: " + horarios[i]);
                        }

                        // Selección de la sala por parte del usuario:
                        int salaElegida;
                        do {
                            System.out.print("Seleccione una sala (1-3): ");
                            salaElegida = scanner.nextInt();
                            scanner.nextLine(); // Limpieza del buffer.
                            if (salaElegida < 1 || salaElegida > 3) {
                                System.out.println("Sala inválida. Seleccione una sala entre 1 y 3.");
                            }
                        } while (salaElegida < 1 || salaElegida > 3);

                        // Mostrar los asientos disponibles en la sala seleccionada.
                        // Se muestran los números de las columnas primero (1 a 10):
                        System.out.println("\n--- Disponibilidad de Asientos ---");
                        System.out.print("   "); // Espacio para alinear con la letra de la fila.
                        for (int j = 1; j <= 10; j++) {
                            System.out.print(j + " ");
                        }
                        System.out.println();
                        
                        // Bucle para recorrer cada una de las 6 filas en la sala.
                        for (int f = 0; f < 6; f++) {
                            // Se convierte el índice de fila a su correspondiente letra (A-F).
                            // Por ejemplo, f = 0 convierte a 'A', f = 1 convierte a 'B', etc.
                            char fila = (char) ('A' + f); 
                            // Se imprime la letra de la fila seguida de dos espacios para una correcta alineación.
                            System.out.print(fila + "  ");
                            
                            // Bucle interno: recorre cada asiento de la fila actual.
                            for (int a = 0; a < 10; a++) {
                                // Se accede a la sala seleccionada: salaElegida - 1 (resta 1 porque los índices comienzan en 0)
                                // y se imprime el estado de cada asiento ('O' para libre, 'X' para ocupado).
                                System.out.print(salas[salaElegida - 1][f][a] + " ");
                            }
                            // Al terminar una fila, se hace un salto de línea.
                            System.out.println();
                        }

                        // Solicitar al usuario la cantidad de asientos que desea comprar.
                        System.out.print("¿Cuántos asientos desea comprar?: ");
                        int cantidadAsientos = scanner.nextInt();
                        scanner.nextLine();
                        
                        // Se declara un arreglo de cadenas para almacenar los asientos reservados.
                        // Cada elemento del arreglo guardará un código, por ejemplo "A5" o "C3".
                        String[] asientosReservados = new String[cantidadAsientos];

                        // Bucle para procesar la reserva de cada asiento.
                        for (int i = 0; i < cantidadAsientos; i++) {
                            boolean asientoValido = false; // Controla si el asiento ingresado es correcto.
                            while (!asientoValido) {
                                // Se pide al usuario que ingrese la fila, la entrada se lee como token, se convierte a mayúsculas y se toma el primer carácter.
                                System.out.print("Ingrese la fila (A-F) para el asiento " + (i + 1) + ": ");
                                char fila = scanner.next().toUpperCase().charAt(0);
                                // Se calcula el índice de la fila restando 'A' al carácter ingresado.
                                int filaIdx = fila - 'A';
                                // Se solicita el número de asiento dentro de la fila.
                                System.out.print("Ingrese el número del asiento (1-10): ");
                                int numAsiento = scanner.nextInt();
                                scanner.nextLine(); // Limpieza del buffer.
                                
                                // Se valida que la letra de la fila y el número sean correctos (dentro del rango permitido).
                                if (fila < 'A' || fila > 'F' || numAsiento < 1 || numAsiento > 10) {
                                    System.out.println("Posición inválida. Intente nuevamente.");
                                    continue; // Se vuelve al inicio del bucle para pedir nuevamente.
                                }
                                // Se verifica si el asiento ya está reservado (marcado con 'X').
                                if (salas[salaElegida - 1][filaIdx][numAsiento - 1] == 'X') {
                                    System.out.println("Asiento ocupado. Seleccione otro.");
                                } else {
                                    // Se marca el asiento como ocupado asignando la 'X'.
                                    salas[salaElegida - 1][filaIdx][numAsiento - 1] = 'X';
                                    // Se guarda el código del asiento (por ejemplo, "A5") en el arreglo.
                                    asientosReservados[i] = fila + "" + numAsiento;
                                    asientoValido = true; // El asiento es válido, se sale del bucle interno.
                                }
                            }
                        }
                        
                        /* 
                         * Nota: Hasta este punto, se han reservado los asientos y calculado el costo de las entradas.
                         * Aún NO se ha actualizado el reporte de ventas para evitar contabilizar compras que luego se cancelen.
                         */
                        
                        // Calcular el total de la venta de entradas.
                        double totalEntradas = precios[salaElegida - 1] * cantidadAsientos;
                        
                        // Proceso de compra de snacks (opcional):
                        System.out.print("¿Desea comprar snacks? (si/no): ");
                        String respuestaSnack = scanner.nextLine();
                        double totalSnack = 0; // Inicializa el total a pagar por snacks.
                        // Se utiliza StringBuilder para concatenar los detalles de cada snack de forma eficiente.
                        StringBuilder detalleSnack = new StringBuilder();
                        
                        // Si el usuario desea comprar snacks:
                        if (respuestaSnack.equalsIgnoreCase("si")) {
                            boolean agregarSnack = true;
                            // Bucle para procesar múltiples compras de snack hasta que el usuario decida detenerse.
                            while (agregarSnack) {
                                System.out.println("\nOpciones de snacks:");
                                // Mostrar el listado de snacks disponibles junto con sus precios.
                                for (int i = 0; i < snacks.length; i++) {
                                    System.out.println((i + 1) + ". " + snacks[i] + " - $" + snackPrecios[i]);
                                }
                                int seleccionSnack;
                                // Se solicita la selección de un snack específico, validando que sea un número entre 1 y 5.
                                do {
                                    System.out.print("Seleccione el snack (1-5): ");
                                    seleccionSnack = scanner.nextInt();
                                    if (seleccionSnack < 1 || seleccionSnack > 5) {
                                        System.out.println("Opción de snack inválida.");
                                    }
                                } while (seleccionSnack < 1 || seleccionSnack > 5);
                                System.out.print("¿Cuántas unidades desea?: ");
                                int unidadesSnack = scanner.nextInt();
                                scanner.nextLine();  // Limpia el buffer.
                                
                                // Calcula el costo del snack seleccionado multiplicando el precio por la cantidad.
                                double costoSnack = snackPrecios[seleccionSnack - 1] * unidadesSnack;
                                // Suma el costo actual al total de snacks.
                                totalSnack += costoSnack;
                                // Actualiza el conteo de snacks vendidos para el tipo seleccionado.
                                snacksVendidos[seleccionSnack - 1] += unidadesSnack;
                                // Agrega el detalle del snack comprado al StringBuilder para impresión posterior.
                                detalleSnack.append(snacks[seleccionSnack - 1])
                                            .append(" x").append(unidadesSnack)
                                            .append(" - $").append(String.format("%.2f", costoSnack))
                                            .append("\n");
                                
                                // Pregunta si se desea agregar otro snack.
                                System.out.print("¿Desea agregar otro snack? (si/no): ");
                                String masSnack = scanner.nextLine();
                                if (!masSnack.equalsIgnoreCase("si")) {
                                    agregarSnack = false;
                                }
                            }
                        }
                        
                        // Calcular los totales de la compra:
                        double subtotal = totalEntradas + totalSnack;   // Suma de entradas y snacks
                        double iva = subtotal * 0.15;                     // IVA del 15%
                        double totalCompra = subtotal + iva;              // Total final a pagar
                        // Se obtiene la fecha actual y se formatea en el patrón "dd/MM/yyyy".
                        String fecha = LocalDate.now().format(DateTimeFormatter.ofPattern("dd/MM/yyyy"));
                        
                        // Preguntar al usuario si desea continuar con el pago o cancelar la compra.
                        System.out.print("\n¿Desea continuar con el pago? (si/no): ");
                        String continuarPago = scanner.nextLine();
                        if (!continuarPago.equalsIgnoreCase("si")) {
                            // Si se cancela el pago, se debe revertir la reserva de asientos.
                            for (String asiento : asientosReservados) {
                                // Se extrae la letra de la fila, por ejemplo 'A'.
                                char row = asiento.charAt(0);
                                // Convertir la letra a índice (A -> 0, B -> 1, etc.).
                                int rowIdx = row - 'A';
                                // Se extrae el número de asiento desde la parte numérica de la cadena.
                                int seatNum = Integer.parseInt(asiento.substring(1));
                                // Liberar el asiento reasignando 'O', indicando que el asiento queda disponible.
                                salas[salaElegida - 1][rowIdx][seatNum - 1] = 'O';
                            }
                            // Se informa que el pago ha sido cancelado y se retorna al menú principal.
                            System.out.println("Pago cancelado. Regresando al menú principal...");
                            break; // Sale del do-while de compra y vuelve al menú principal.
                        }
                        
                        // Solo si se confirma el pago, se actualizan los contadores de ventas:
                        // Se suman los boletos vendidos y los ingresos correspondientes a la sala.
                        boletosVendidos[salaElegida - 1] += cantidadAsientos;
                        ingresosPorSala[salaElegida - 1] += totalEntradas;
                        
                        // PROCESO DE PAGO: Se solicita el método de pago al usuario.
                        System.out.println("\n========= MÉTODO DE PAGO =========");
                        System.out.print("Seleccione método de pago (1: Efectivo, 2: Tarjeta): ");
                        int metodoPago = scanner.nextInt();
                        scanner.nextLine();  // Limpieza del buffer.
                        
                        String mensajePago = "";  // Variable para guardar el mensaje de confirmación del pago.
                        String nombreFactura = "";  // Se usará para almacenar el nombre del titular en caso de pago.
                        String direccion = "";      // Para almacenar la dirección (en pago en efectivo).
                        String correo = "";         // Para almacenar el correo.
                        
                        // Proceso para pago en efectivo:
                        if (metodoPago == 1) {
                            System.out.print("Ingrese sus nombres completos: ");
                            nombreFactura = scanner.nextLine();
                            System.out.print("Ingrese su dirección: ");
                            direccion = scanner.nextLine();
                            System.out.print("Ingrese su correo: ");
                            correo = scanner.nextLine();
                            Random rand = new Random();
                            // Se genera un código aleatorio entre 1000 y 9999.
                            int codigo = 1000 + rand.nextInt(9000);
                            // Se construye el mensaje de pago.
                            mensajePago = "Acérquese a caja con el código: " + codigo + " para cancelar su compra.";
                        }
                        // Proceso para pago con tarjeta:
                        else if (metodoPago == 2) {
                            System.out.print("Ingrese el nombre del titular: ");
                            nombreFactura = scanner.nextLine();
                            String tarjeta;
                            // Se valida que la tarjeta ingresada tenga exactamente 10 dígitos.
                            do {
                                System.out.print("Ingrese los 10 dígitos de la tarjeta: ");
                                tarjeta = scanner.nextLine();
                            } while (tarjeta.length() != 10);
                            String vencimiento;
                            // Se valida el formato del vencimiento (MM/AA), asegurándose de que el mes esté entre 01 y 12.
                            do {
                                System.out.print("Ingrese la fecha de vencimiento (MM/AA): ");
                                vencimiento = scanner.nextLine();
                            } while (!vencimiento.matches("(0[1-9]|1[0-2])/\\d{2}"));
                            String cvv;
                            // Se valida que el CVV tenga exactamente 3 dígitos.
                            do {
                                System.out.print("Ingrese el CVV (3 dígitos): ");
                                cvv = scanner.nextLine();
                            } while (!cvv.matches("\\d{3}"));
                            System.out.print("Ingrese su correo electrónico: ");
                            correo = scanner.nextLine();
                            mensajePago = "Pago con tarjeta finalizado con éxito. Su factura electrónica fue enviada al correo: " + correo + ".";
                        } else {
                            // Si el método de pago ingresado no es válido, se cancela la compra.
                            System.out.println("Método de pago inválido. Cancelando la compra.");
                            break;
                        }
                        
                        // IMPRESIÓN DE LA FACTURA:
                        // Se muestran en detalle todos los datos de la compra, incluyendo fecha, asientos, totales, etc.
                        System.out.println("\n======================= FACTURA =======================");
                        System.out.println("Fecha: " + fecha);
                        // Muestra el nombre del titular, si se ingresó; de lo contrario, muestra "Sin dato".
                        System.out.println("Cliente: " + (nombreFactura.isEmpty() ? "Sin dato" : nombreFactura));
                        // Solo se muestra la dirección si no está vacía.
                        if (!direccion.isEmpty()) {
                            System.out.println("Dirección: " + direccion);
                        }
                        System.out.println("Correo: " + correo);
                        // Se muestra la película seleccionada, según la sala elegida.
                        System.out.println("Película: " + peliculas[salaElegida - 1]);
                        System.out.println("Sala: " + salaElegida + " - " + tipos[salaElegida - 1]);
                        System.out.print("Asientos: ");
                        // Se recorren y muestran todos los asientos reservados.
                        for (String asiento : asientosReservados) {
                            System.out.print(asiento + " ");
                        }
                        // Se muestra el desglose del costo: entradas, snacks, subtotal, IVA y total.
                        System.out.println("\nEntradas: $" + String.format("%.2f", totalEntradas));
                        System.out.println("Snacks:\n" + (detalleSnack.length() == 0 ? "-" : detalleSnack.toString()));
                        System.out.println("Subtotal: $" + String.format("%.2f", subtotal));
                        System.out.println("IVA (15%): $" + String.format("%.2f", iva));
                        System.out.println("Total a pagar: $" + String.format("%.2f", totalCompra));
                        System.out.println(mensajePago);
                        System.out.println("=======================================================");
                        
                        // Preguntar al usuario si desea realizar otra compra.
                        // Si se responde "si", el bucle do-while se repetirá y se iniciará un nuevo proceso de compra.
                        System.out.print("\n¿Desea realizar otra compra? (si/no): ");
                        otraCompra = scanner.nextLine();
                        
                    } while (otraCompra.equalsIgnoreCase("si"));
                    
                    // Al finalizar el proceso de compra, se regresa al menú principal.
                    break;
                    
                case 2:
                    // REPORTE DE VENTAS:
                    // Se muestra el reporte solo si el usuario elige esta opción del menú principal.
                    System.out.println("\n========= REPORTE DE VENTAS =========");
                    int totalBoletos = 0;      // Inicializa contador para total de boletos vendidos.
                    double totalRecaudado = 0; // Inicializa contador para total de ingresos.
                    
                    // Se recorre cada sala para mostrar el reporte individual y calcular el total acumulado.
                    for (int i = 0; i < peliculas.length; i++) {
                        System.out.println("Sala " + (i + 1) + " - " + peliculas[i]);
                        System.out.println("  Boletos vendidos: " + boletosVendidos[i]);
                        System.out.println("  Total recaudado: $" + String.format("%.2f", ingresosPorSala[i]));
                        totalBoletos += boletosVendidos[i];          // Suma los boletos vendidos en la sala actual.
                        totalRecaudado += ingresosPorSala[i];         // Suma los ingresos de la sala actual.
                    }
                    // Se imprime el total acumulado para todas las salas.
                    System.out.println("--------------------------------------");
                    System.out.println("Total de boletos vendidos en todas las salas: " + totalBoletos);
                    System.out.println("Total recaudado en todas las salas: $" + String.format("%.2f", totalRecaudado));
                    System.out.println("======================================");
                    break;
                    
                case 3:
                    // Opción para salir del programa.
                    salir = true;
                    break;
                    
                default:
                    // Opción no válida ingresada por el usuario.
                    System.out.println("Opción inválida. Intente nuevamente.");
            }
        }
        
        // Mensaje final de despedida al salir del programa.
        System.out.println("\nGracias por visitar CodeCine. ¡Hasta pronto!");
        // Se cierra el objeto Scanner para liberar recursos.
        scanner.close();
    }
}
