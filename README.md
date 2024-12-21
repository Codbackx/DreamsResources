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
            func displaySuccessAlert(viewController: UIViewController, title: String, message: String, okActionHandler: @escaping () -> Void) {
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
### Para hacer uso de UserDefaults

  #### Para Guardar Informacion (value - key)
    UserDefaults.standard.set(userLogged.fullNames, forKey: "fullNamesOfUser")
    UserDefaults.standard.set(userLogged.email, forKey: "emailOfUser")
    UserDefaults.standard.set(userLogged.dniNumer, forKey: "dniNumberOfUser")
    UserDefaults.standard.set(true, forKey: "existToSession")
  #### Para Leer informacion
    let fullnames = UserDefaults.standard.string(forKey: "fullNamesOfUser")
    let email = UserDefaults.standard.string(forKey: "emailOfUser")
    let dniNumber = UserDefaults.standard.string(forKey: "dniNumberOfUser")
    let exitToSession = UserDefaults.standard.bool(forKey: "existToSession")
### Para buscar por contenido 
    let db = Firestore.firestore()
    let searchText = "adm" // Subcadena a buscar

    db.collection("users").getDocuments { (querySnapshot, error) in
      if let error = error {
        print("Error obteniendo documentos: \(error)")
      } else {
        guard let documents = querySnapshot?.documents else { return }
        let filteredDocuments = documents.filter { document in
            if let roles = document.data()["roles"] as? [String] {
                return roles.contains(where: { $0.contains(searchText) })
            }
            return false
        }
        
        // Imprimir documentos que coincidan
        for document in filteredDocuments {
            print("\(document.documentID) => \(document.data())")
        }
      }
    }

### Convertir un texfield en un campo para poner fecha
  #### Clase auxiliar
    import UIKit

    class DatePickerHelper {
    static func configureDatePicker(for textField: UITextField, dateFormat: String = "dd/MM/yyyy", doneTitle: String = "Hecho") {
        // Crear el DatePicker
        let datePicker = UIDatePicker()
        datePicker.datePickerMode = .date
        
        // Configuración para dispositivos con iOS 14 o posterior
        if #available(iOS 14, *) {
            datePicker.preferredDatePickerStyle = .wheels
        }
        
        // Añadir una barra de herramientas con un botón "Hecho"
        let toolbar = UIToolbar()
        toolbar.sizeToFit()
        
        let doneButton = UIBarButtonItem(title: doneTitle, style: .done, target: textField, action:#selector(textField.resignFirstResponder))
        toolbar.setItems([doneButton], animated: true)
        
        textField.inputAccessoryView = toolbar
        textField.inputView = datePicker
        
        datePicker.addTarget(self, action: #selector(dateChanged(datePicker:for:)), for: .valueChanged)
        
        objc_setAssociatedObject(textField, &dateFormatterKey, dateFormat, .OBJC_ASSOCIATION_RETAIN_NONATOMIC)
    }
    
    @objc private static func dateChanged(datePicker: UIDatePicker, for textField: UITextField) {
        guard let dateFormat = objc_getAssociatedObject(textField, &dateFormatterKey) as? String else { return }
        let formatter = DateFormatter()
        formatter.dateFormat = dateFormat
        textField.text = formatter.string(from: datePicker.date)
      }
    }

    private var dateFormatterKey: UInt8 = 0

  #### Ejemplo de como usarla

     @IBOutlet weak var dateTextField: UITextField!
      @IBOutlet weak var anotherDateTextField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Configurar los campos de texto para que usen el DatePicker
        DatePickerHelper.configureDatePicker(for: dateTextField, dateFormat: "dd/MM/yyyy")
        DatePickerHelper.configureDatePicker(for: anotherDateTextField, dateFormat: "yyyy-MM-dd", doneTitle: "Aceptar")
    }



### Funcion para hacer que se haga algo mientras se escriba en un textfield

      @objc func textFieldDidChange(_ textField: UITextField) {
        // Acción que se ejecuta cuando el texto cambia
        print("El texto actual es: \(textField.text ?? "")")
        
      }

      //esto se pone en el viewDidLoad 
      sampleTextField.addTarget(self, action: #selector(textFieldDidChange(_:)), for: .editingChanged)






  
  
    
  



