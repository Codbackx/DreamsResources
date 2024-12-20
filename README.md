## Dreams iOS

### Estos son los recuros necesarios para construir la UI de la aplicacion

### Diseño (Figma): [DreamsDesign](https://www.figma.com/design/dONuMtUIZmygACBVlPtFVR/DreamsApp?node-id=0-1&t=mLbo6wXoyyLgGIrH-1)

### Puntos a desarrollar el dia de hoy 
  ## Importantes
    - Filtrar habitaciones por contenido
    - Guardar informacion del ususario logeado
    - Mostrar informacion del usuario logeado en la seccion de perfil y reservas
    - Guardar y listar habitaciones favoritas en coredata
    - Cambiar estado de la habitacion al ser reservada (OCCUPY - AVAILABLE)
    - Validar fechas
    - Campos de fecha
    - Funciones para (Verificar si es mayor de edad segun la fecha de nacimiento, )
  ## Menos importantes
    - Agregar alertas para cada accion (reservar,guardar habitacion, registrarse)
  ## Opcionales
    - Mostrar carga animada
    - En caso de vacio mostrar texto de vacio



### ! Funciones
  #### Para verificar si es mayor de edad
          func calculateAge(from birthDateString: String) -> Bool? {
            let dateFormatter = DateFormatter()
            dateFormatter.dateFormat = "dd/MM/yyyy"
            guard let birthDate = dateFormatter.date(from: birthDateString) else { return nil }
            let currentAge : Int? = Calendar.current.dateComponents([.year], from: birthDate, to: Date()).year

            return currentAge! >= 18
          }
  #### Para calcular la cantidad de dias de hospedaje
          func calculateHostedDay(from startDateString: String, to endDateString: String, with format: String = "dd/MM/yyyy") -> Int? {
            let dateFormatter = DateFormatter()
            dateFormatter.dateFormat = format

            guard let startDate = dateFormatter.date(from: startDateString),
              let endDate = dateFormatter.date(from: endDateString) else {
              return nil
            }

            let difference = Calendar.current.dateComponents([.day], from: startDate, to: endDate)
            return difference.day
            }
  #### Para validar fechas de reservas
          
          func validateDateReservation(entrada: String, salida: String) -> String? {
            let fechaActual = Date()
            let formatter = DateFormatter()
            formatter.dateFormat = "dd/MM/yyyy"
            let fechaEntradaFormated = formatter.date(from: entrada)!
            let fechaSalidaFormated = formatter.date(from: salida)!
    
            if fechaEntradaFormated < fechaActual {
              return "La fecha de entrada no puede ser menor a la fecha actual."
            }
    
    
            if fechaEntradaFormated > fechaSalidaFormated {
              return "La fecha de entrada no puede ser mayor a la fecha de salida."
            }
    
            return nil
            }
### ! Alertas
  ### Alerta de confirmacion
            func displaySuccessAlert(viewController: UIViewController, title: String, message: String, okActionHandler: @escaping () ->               Void) {
              let alerta = UIAlertController(title: title, message: message, preferredStyle: .alert)
  
              let okAction = UIAlertAction(title: "OK", style: .default) { _ in
                okActionHandler()
              }
    
              alerta.addAction(okAction)
    
              viewController.present(alerta, animated: true, completion: nil)
            }

  ### Alerta de error
          func displayErrorAlert(viewController: UIViewController, title : String, message: String) {
            let alerta = UIAlertController(title: title, message: message, preferredStyle: .alert)
    
            let okAction = UIAlertAction(title: "OK", style: .default, handler: nil)
    
            alerta.addAction(okAction)
    
            viewController.present(alerta, animated: true)
          }
  
    
  



