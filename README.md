## Dreams iOS

### Estos son los recuros necesarios para construir la UI de la aplicacion

### Diseño (Figma): [DreamsDesign](https://www.figma.com/design/dONuMtUIZmygACBVlPtFVR/DreamsApp?node-id=0-1&t=mLbo6wXoyyLgGIrH-1)

### Simbolos de texto: 
- ⓘ

### Colores HEX y RGB
- ![Static Badge](https://img.shields.io/badge/%23FFEA04%20%7C%7C%20255%2C%20234%2C%204-FFEA04)
- ![Static Badge](https://img.shields.io/badge/%23262626%20%7C%7C%2038%2C%2038%2C%2038-262626)
- ![Static Badge](https://img.shields.io/badge/%23A4A4A4%20%7C%7C%20164%2C%20164%2C%20164-A4A4A4)
- ![Static Badge](https://img.shields.io/badge/%235D5D5D%20%7C%7C%2093%2C%2093%2C%2093-5D5D5D)
- ![Static Badge](https://img.shields.io/badge/%23D1D1D1%20%7C%7C%20RGB(209%2C%20209%2C%20209)-%23D1D1D1)
- ![Static Badge](https://img.shields.io/badge/%233E3C22%20%7C%7C%2062%2C%2060%2C%2034-3E3C22)
- ![Static Badge](https://img.shields.io/badge/%23FCF6B3%20%7C%7C%20252%2C%20246%2C%20179-FCF6B3)
- ![Static Badge](https://img.shields.io/badge/%23FCF06F%20%7C%7C%20255%2C%20234%2C%204-FCF06F)
- ![Static Badge](https://img.shields.io/badge/%23000000%20%7C%7C%200%2C%200%2C%200-000000)
- ![Static Badge](https://img.shields.io/badge/%23FFFFFF%20%7C%7C%20255%2C%20255%2C%20255-FFFFFF)


### Modelos Requeridos

- RoomEntity {
    images : [String],
    price : Float,
    description : String,
    status : StatusRoomEnum (OCCUPY, AVAILABLE),
    number : UInt
} 

- ReservationEntity {
    userDni : String,
    numberRoom : UInt,
    entryDate : String,
    departureDate : String,
    dateReservation : String
}

- UserEnity{
    dniNumber : String,
    fullNames : String,
    lastNames : String,
    birthdayDate : String,
    phoneNumber : String,
    password : String
} 


- BillEntity{userDni : String,amountTotal : Double}






