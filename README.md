# Trabajo final 
## Codigo 
### ThisWorkbook
```
Option Explicit

Private Sub WorkBook_Open()
  Application.Caption = "Baloto"
  UserForm1.Show (1)
  
  ' Configurar los encabezados en la fila 1
    Dim ws As Worksheet
    Set ws = ThisWorkbook.Sheets("Hoja3")
 
    
    ' Insertar los encabezados en la fila 1
    ws.Cells(1, 1).Value = "ID"
    ws.Cells(1, 2).Value = "Boleta"
    ws.Cells(1, 3).Value = "Número1"
    ws.Cells(1, 4).Value = "Número2"
    ws.Cells(1, 5).Value = "Número3"
    ws.Cells(1, 6).Value = "Número4"
    ws.Cells(1, 7).Value = "Número5"
    ws.Cells(1, 8).Value = "Número6"
    ws.Cells(1, 9).Value = "balota"
    
    
    ' Opcional: Formato de los encabezados (negrita)
    ws.Rows(1).Font.Bold = True
End Sub
```

### UserForm1
```
Option Explicit

Private Function BoletaRegistrada(boletaSeleccionada As String) As Boolean
    Dim ws As Worksheet
    Dim lastRow As Long
    Dim i As Long
    
    ' Definir la hoja de cálculo donde se ingresaron las boletas
    Set ws = ThisWorkbook.Sheets("Hoja3") ' Cambia "Hoja3" si tu hoja tiene otro nombre
    
    ' Encontrar la última fila con datos en la columna B
    lastRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row
    
    ' Recorrer todas las filas de la columna B para verificar si la boleta existe
    For i = 1 To lastRow
        If ws.Cells(i, 2).Value = boletaSeleccionada Then
            BoletaRegistrada = True
            Exit Function
        End If
    Next i
    
    ' Si no se encontró la boleta, retornar False
    BoletaRegistrada = False
End Function



Private Sub UserForm_Initialize()
    ' Llenar los cuadros combinados con números del 0 al 9
    Dim i As Integer
    Dim randomNumber As Integer
    Dim numbersUsed As Collection
    For i = 1 To 43
        ComboBox1.AddItem i
        ComboBox2.AddItem i
        ComboBox3.AddItem i
        ComboBox4.AddItem i
        ComboBox5.AddItem i
        ComboBox6.AddItem i
    Next i
    
    For i = 1 To 16
        ComboBalota.AddItem i
    Next i
    
    ' --------------------------------
    ' Crear una colección para rastrear los números ya asignados
    Set numbersUsed = New Collection

    ' Generar y asignar un número aleatorio único a ComboBox1
    Do
        randomNumber = Int((43) * Rnd) ' Número aleatorio entre 0 y 9
        On Error Resume Next ' Ignorar errores si el número ya existe
        numbersUsed.Add randomNumber, CStr(randomNumber) ' Añadir número a la colección
        On Error GoTo 0
    Loop Until numbersUsed.Count = 1
    ComboBox1.Value = randomNumber

    ' Generar y asignar un número aleatorio único a ComboBox2
    Do
        randomNumber = Int((43) * Rnd)
        On Error Resume Next
        numbersUsed.Add randomNumber, CStr(randomNumber)
        On Error GoTo 0
    Loop Until numbersUsed.Count = 2
    ComboBox2.Value = randomNumber

    ' Generar y asignar un número aleatorio único a ComboBox3
    Do
        randomNumber = Int((43) * Rnd)
        On Error Resume Next
        numbersUsed.Add randomNumber, CStr(randomNumber)
        On Error GoTo 0
    Loop Until numbersUsed.Count = 3
    ComboBox3.Value = randomNumber
    
    Do
        randomNumber = Int((43) * Rnd)
        On Error Resume Next
        numbersUsed.Add randomNumber, CStr(randomNumber)
        On Error GoTo 0
    Loop Until numbersUsed.Count = 4
    ComboBox4.Value = randomNumber
    
    Do
        randomNumber = Int((43) * Rnd)
        On Error Resume Next
        numbersUsed.Add randomNumber, CStr(randomNumber)
        On Error GoTo 0
    Loop Until numbersUsed.Count = 5
    ComboBox5.Value = randomNumber
    
    Do
        randomNumber = Int((43) * Rnd)
        On Error Resume Next
        numbersUsed.Add randomNumber, CStr(randomNumber)
        On Error GoTo 0
    Loop Until numbersUsed.Count = 6
    ComboBox6.Value = randomNumber
    
    
    ' Generar y asignar un número aleatorio entre 0 y 9 a cada ComboBox al cargar el formulario
    randomNumber = Int((16) * Rnd) ' Número aleatorio entre 0 y 9
    ComboBalota.Value = randomNumber
    '--------------------------------
    
    ' Llenar ComboBox de Boletas desde la columna B
    CargarBoletas
End Sub

Private Sub CargarBoletas()
    Dim lastRow As Long
    Dim ws As Worksheet
    Dim i As Long
    Dim tieneBoletas As Boolean
    
    ' Definir la hoja de cálculo que contiene las boletas
    Set ws = ThisWorkbook.Sheets("Hoja3") ' Cambia "NombreDeTuHoja" por el nombre de tu hoja de cálculo
    
    ' Encontrar la última fila con datos en la columna B
    lastRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row
    
    ' Limpiar el cuadro combinado antes de agregar nuevas opciones
    ComboBoxBoleta.Clear
    tieneBoletas = False
    
    ' Comprobar si hay boletas en la columna B
    For i = 1 To lastRow
        If ws.Cells(i, 2).Value <> "" Then ' Columna B es la columna 2
            ComboBoxBoleta.AddItem ws.Cells(i, 2).Value
            tieneBoletas = True
        End If
    Next i
    
    ' Si no hay boletas, mostrar un mensaje en el ComboBox
    If Not tieneBoletas Then
        ComboBoxBoleta.AddItem "No se han ingresado boletas"
    End If
End Sub

Private Sub btnIngresar_Click()
    Dim ws As Worksheet
    Dim nextRow As Long
    Dim nuevaBoleta As String
    Dim lastRow As Long
    Dim i As Long
    Dim boletaExiste As Boolean
    Dim numeros(1 To 7) As String
    Dim j As Long, k As Long
    Dim duplicado As Boolean
    
    ' Definir la hoja de cálculo donde se ingresarán las boletas
    Set ws = ThisWorkbook.Sheets("Hoja3") ' Cambia "Hoja3" por el nombre de tu hoja de Excel
    
    ' Obtener los números seleccionados de los ComboBox
    numeros(1) = ComboBox1.Value
    numeros(2) = ComboBox2.Value
    numeros(3) = ComboBox3.Value
    numeros(4) = ComboBox4.Value
    numeros(5) = ComboBox5.Value
    numeros(6) = ComboBox6.Value
    numeros(7) = ComboBalota.Value
    
    ' Validar que no haya campos vacíos
    For i = 1 To 7
        If numeros(i) = "" Then
            MsgBox "Todos los campos deben tener un número seleccionado.", vbExclamation
            Exit Sub
        End If
    Next i
    
    ' Verificar si hay números duplicados en los ComboBox
    duplicado = False
    For j = 1 To 6
        For k = j + 1 To 6
            If numeros(j) = numeros(k) Then
                duplicado = True
                Exit For
            End If
        Next k
        If duplicado Then Exit For
    Next j
    
    ' Si hay números duplicados, mostrar un mensaje y salir
    If duplicado Then
        MsgBox "No se permite ingresar números duplicados en los ComboBox. Por favor, seleccione números diferentes.", vbExclamation
        Exit Sub
    End If
    
    ' Crear la boleta combinando los números seleccionados en los ComboBox
    nuevaBoleta = numeros(1) & numeros(2) & numeros(3) & numeros(4) & numeros(5) & numeros(6) & numeros(7)
    
    ' Encontrar la última fila con datos en la columna B
    lastRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row
    
    ' Si no existe, encontrar la siguiente fila vacía en la columna B
    nextRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row + 1
    
    ' Ingresar el número de boleta en la columna A (secuencial)
    ws.Cells(nextRow, 1).Value = nextRow - 1 ' Restamos 1 ya que la primera fila tiene los encabezados
    
    ' Ingresar la nueva boleta en la siguiente fila vacía de la columna B
    ws.Cells(nextRow, 2).Value = nuevaBoleta
    ws.Cells(nextRow, 3).Value = numeros(1)
    ws.Cells(nextRow, 4).Value = numeros(2)
    ws.Cells(nextRow, 5).Value = numeros(3)
    ws.Cells(nextRow, 6).Value = numeros(4)
    ws.Cells(nextRow, 7).Value = numeros(5)
    ws.Cells(nextRow, 8).Value = numeros(6)
    ws.Cells(nextRow, 9).Value = numeros(7)
    
    ' Confirmar que la boleta se ha ingresado
    MsgBox "Boleta ingresada: " & nuevaBoleta, vbInformation
    
    ' Actualizar el ComboBox de boletas
    CargarBoletas
End Sub



Private Sub btnJugar_Click()
    Dim boletaSeleccionada As String
    
    ' Obtener la boleta seleccionada en el ComboBox
    boletaSeleccionada = ComboBoxBoleta.Value
    
    ' Obtener la boleta seleccionada en el ComboBox y guardada en la global
    boletaEscogidaGlobal = ComboBoxBoleta.Value
    
    ' Verificar si la boleta seleccionada está registrada en la columna B
    If Not BoletaRegistrada(boletaSeleccionada) Then
        MsgBox "La boleta seleccionada no está registrada. Por favor, seleccione una boleta válida.", vbExclamation
        Exit Sub
    End If
    MsgBox "Tu voleta seleccionada fue: " & boletaSeleccionada
    MsgBox "Tu voleta seleccionada fue: " & boletaEscogidaGlobal
    ' Si la boleta es válida, cerrar el formulario y abrir el siguiente formulario
    Me.Hide
    UserForm2.Show ' Asumiendo que UserForm2 es el nombre del segundo formulario
    
End Sub


Private Sub Salir_Click()
    ' Guardar el archivo sin mostrar cuadro de diálogo
    ThisWorkbook.Save
    
    ' Cerrar Excel sin mostrar mensaje de confirmación
    Application.Quit
End Sub
```

### UserForm2
```
Option Explicit

Private Sub Regresar_Click()
' Cerrar el formulario actual
    Unload Me
    
    ' Abrir el formulario anterior (UserForm1)
    UserForm1.Show
End Sub


Function BuscarBoleta(ByVal boleta As String) As Variant
    Dim ws As Worksheet
    Dim lastRow As Long
    Dim i As Long
    Dim resultado(1 To 2) As Variant ' Array que devolverá la fila y el valor de la columna A
    MsgBox "boleta" & boleta
    
    ' Definir la hoja de cálculo
    Set ws = ThisWorkbook.Sheets("Hoja3") ' Cambia "Hoja3" si tu hoja tiene otro nombre
    
    ' Encontrar la última fila con datos en la columna B
    lastRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row
    
    ' Recorrer la columna B para buscar la boleta seleccionada
    For i = 2 To lastRow ' Asumimos que la fila 1 tiene encabezados
        If ws.Cells(i, 2).Value = boleta Then
            ' Si encuentra la boleta, guarda la fila y el valor de la columna A
            resultado(1) = i ' Fila donde está la boleta
            resultado(2) = ws.Cells(i, 1).Value ' Número en la columna A
            BuscarBoleta = resultado ' Devolver array con la fila y el valor en la columna A
            Exit Function
        End If
    Next i
    
    ' Si no se encuentra la boleta, devolver un array vacío
    BuscarBoleta = Array(0, "No encontrada")
    MsgBox "No hay nada"
End Function



Private Sub btnObtenerGanador_Click()
    Dim numerosGanadores(1 To 7) As Integer
    Dim i As Integer
    Dim numAleatorio As Integer
    Dim j As Integer
    Dim existe As Boolean
    Dim boletaGanadora As String
    ' Inicializar el generador de números aleatorios
    Randomize
    
    ' Generar 6 números únicos entre 1 y 43
    For i = 1 To 6
        Do
            existe = False
            numAleatorio = Int((43 - 1 + 1) * Rnd + 1) ' Generar número entre 1 y 43
            
            ' Verificar si el número ya ha sido generado
            For j = 1 To i - 1
                If numerosGanadores(j) = numAleatorio Then
                    existe = True
                    Exit For
                End If
            Next j
        Loop While existe ' Volver a intentar si el número ya existe
        
        ' Asignar el número único a la posición correspondiente
        numerosGanadores(i) = numAleatorio
    Next i
    numAleatorio = Int((16 - 1 + 1) * Rnd + 1)
    numerosGanadores(7) = numAleatorio
    
    ' Asignar los números ganadores a las TextBox correspondientes
    TextBoxNroGanador1.Value = numerosGanadores(1)
    TextBoxNroGanador2.Value = numerosGanadores(2)
    TextBoxNroGanador3.Value = numerosGanadores(3)
    TextBoxNroGanador4.Value = numerosGanadores(4)
    TextBoxNroGanador5.Value = numerosGanadores(5)
    TextBoxNroGanador6.Value = numerosGanadores(6)
    TextBoxBalota.Value = numerosGanadores(7)
    
    boletaGanadora = numerosGanadores(1) & numerosGanadores(2) & numerosGanadores(3) & numerosGanadores(4) & numerosGanadores(5) & numerosGanadores(6) & numerosGanadores(7)
    TextBoxBaloto.Value = boletaGanadora
    ' Mensaje para confirmar que se generaron los números ganadores
    MsgBox "Números ganadores generados exitosamente.", vbInformation
End Sub

Private Sub Verificarganador_Click()
    Dim ws As Worksheet
    Dim lastRow As Long
    Dim i As Long, j As Long
    Dim ganadores(1 To 6) As String
    Dim ganadortotal As String
    Dim ganadores6 As Collection
    Dim ganadores5 As Collection
    Dim ganadores6ybalota As Collection
    Dim ganadores5ybalota As Collection
    Dim coincidenciasTotales As String
    Dim coincidenciasCinco As String
    Dim coincidencias As Integer
    Dim coincidenciabalota As Integer
    Dim coincidenciaEncontrada As Boolean
    Dim coincidenciatotalEncontrada As Boolean
    Dim cincoAciertosEncontrado As Boolean
    Dim resultado As Variant
    Dim filaBoleta As Long
    Dim numeroColumnaA As String
     Dim encontrado As Boolean
    
    ' Definir la hoja de cálculo donde están las boletas
    Set ws = ThisWorkbook.Sheets("Hoja3") ' Cambia "Hoja3" por el nombre de tu hoja
    
    ' Obtener la última fila con datos en la columna B
    lastRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row
    
    ' Guardar los números ganadores desde los TextBox
    ganadores(1) = TextBoxNroGanador1.Value
    ganadores(2) = TextBoxNroGanador2.Value
    ganadores(3) = TextBoxNroGanador3.Value
    ganadores(4) = TextBoxNroGanador4.Value
    ganadores(5) = TextBoxNroGanador5.Value
    ganadores(6) = TextBoxNroGanador6.Value
    ganadortotal = TextBoxBalota.Value
    
    ' Inicializar las variables que almacenarán las coincidencias
    coincidenciasTotales = "Números ganadores encontrados en las siguientes filas con 6 coincidencias:" & vbCrLf
    coincidenciasCinco = "Boletas con 5 números correctos en las siguientes filas:" & vbCrLf
    coincidenciaEncontrada = False
    cincoAciertosEncontrado = False
    
    ' Inicializar colecciones para almacenar los ganadores de 6 y 5 aciertos
    Set ganadores6 = New Collection
    Set ganadores5 = New Collection
    Set ganadores6ybalota = New Collection
    Set ganadores5ybalota = New Collection
    
    ' Recorrer todas las filas de las columnas C a H (las que almacenan los números separados)
    For i = 2 To lastRow ' Comienza desde 2 para evitar el encabezado
        coincidencias = 0
        coincidenciabalota = 0
        ' Comparar cada número de las columnas C a H con los números ganadores
        If ganadores(1) = ws.Cells(i, 3).Value Then coincidencias = coincidencias + 1 ' Columna C
        If ganadores(2) = ws.Cells(i, 4).Value Then coincidencias = coincidencias + 1 ' Columna D
        If ganadores(3) = ws.Cells(i, 5).Value Then coincidencias = coincidencias + 1 ' Columna E
        If ganadores(4) = ws.Cells(i, 6).Value Then coincidencias = coincidencias + 1 ' Columna F
        If ganadores(5) = ws.Cells(i, 7).Value Then coincidencias = coincidencias + 1 ' Columna G
        If ganadores(6) = ws.Cells(i, 8).Value Then coincidencias = coincidencias + 1 ' Columna H
        If ganadortotal = ws.Cells(i, 9).Value Then coincidenciabalota = coincidenciabalota + 1 ' Columna I
        
        If coincidenciabalota = 1 Then
           coincidenciatotalEncontrada = True
           ' Si todos los números coinciden (6 coincidencias), agregar a la lista de coincidencias totales
           If coincidencias = 6 Then
             coincidenciasTotales = coincidenciasTotales & "Fila: " & i & ", Boleta No: " & ws.Cells(i, 1).Value & vbCrLf
             coincidenciaEncontrada = True
             ganadores6ybalota.Add i ' Guardar la fila del ganador de 6 números
           ' Si hay exactamente 5 coincidencias, agregar a la lista de 5 aciertos
           ElseIf coincidencias = 5 Then
             coincidenciasCinco = coincidenciasCinco & "Fila: " & i & ", Boleta No: " & ws.Cells(i, 1).Value & vbCrLf
             cincoAciertosEncontrado = True
             ganadores5ybalota.Add i ' Guardar la fila del ganador de 5 números
           End If
        End If
        If coincidenciabalota = 0 Then
           coincidenciatotalEncontrada = False
           ' Si todos los números coinciden (6 coincidencias), agregar a la lista de coincidencias totales
           If coincidencias = 6 Then
             coincidenciasTotales = coincidenciasTotales & "Fila: " & i & ", Boleta No: " & ws.Cells(i, 1).Value & vbCrLf
             coincidenciaEncontrada = True
             ganadores6.Add i ' Guardar la fila del ganador de 6 números
           ' Si hay exactamente 5 coincidencias, agregar a la lista de 5 aciertos
           ElseIf coincidencias = 5 Then
             coincidenciasCinco = coincidenciasCinco & "Fila: " & i & ", Boleta No: " & ws.Cells(i, 1).Value & vbCrLf
             cincoAciertosEncontrado = True
             ganadores5.Add i ' Guardar la fila del ganador de 5 números
           End If
        End If
    Next i
    
    ' Mostrar el mensaje con ganadores de 6 números
    If coincidenciaEncontrada Then
        If coincidenciabalota Then
           MsgBox coincidenciasTotales, vbInformation, "Ganadores con 6 Números y balota (Ganador total)"
        Else
           MsgBox coincidenciasTotales, vbInformation, "Ganadores con 6 Números Pero no balota"
        End If
    Else
        MsgBox "No se encontraron coincidencias con los 6 números ganadores.", vbExclamation, "Sin Ganadores"
    End If
    
    ' Mostrar el mensaje con los que tienen 5 números correctos
    If cincoAciertosEncontrado Then
        If coincidenciabalota Then
           MsgBox coincidenciasCinco, vbInformation, "Ganadores con 5 Números y balota"
        Else
           MsgBox coincidenciasCinco, vbInformation, "Ganadores con 5 Números pero no balota"
        End If
    Else
        MsgBox "No se encontraron boletas con 5 números correctos.", vbExclamation, "Sin Ganadores de 5 Números"
    End If
    
    '-------------------------------------------------
    
    ' Llamar a la función BuscarBoleta para encontrar la fila y número en la columna A de la boleta seleccionada
    MsgBox "boleta" & boletaEscogidaGlobal
    resultado = BuscarBoleta(boletaEscogidaGlobal)
    
    ' Acceder correctamente a los elementos del resultado
    filaBoleta = CLng(resultado(1)) ' Asegurar que el valor de la fila es un número
    numeroColumnaA = CStr(resultado(2)) ' Asegurar que el número de la columna A es un string
    
    ' Verificar si la boleta seleccionada está en la lista de ganadores de 6 números
    encontrado = False
    For i = 1 To ganadores6.Count
        If ganadores6(i) = filaBoleta Then
            MsgBox "¡Eres un ganador pleno! Tu boleta " & numeroColumnaA & " tiene 6 números correctos pero no la balota.", vbInformation
            encontrado = True
            Exit For
        End If
    Next i
    
    ' Si no ganó con 6 números, verificar si acertó 5 números
    If Not encontrado Then
        For i = 1 To ganadores5.Count
            If ganadores5(i) = filaBoleta Then
                MsgBox "¡Acertaste 5 de 6 números! Tu boleta " & numeroColumnaA & " tiene 5 números correctos pero no la balota.", vbInformation
                encontrado = True
                Exit For
            End If
        Next i
    End If
    
    If Not encontrado Then
        For i = 1 To ganadores6ybalota.Count
            If ganadores6ybalota(i) = filaBoleta Then
                MsgBox "¡Eres un ganador pleno! Tu boleta " & numeroColumnaA & " tiene 6 números correctos y la balota.", vbInformation
                encontrado = True
                Exit For
            End If
        Next i
    End If
    
    If Not encontrado Then
        For i = 1 To ganadores5ybalota.Count
            If ganadores5ybalota(i) = filaBoleta Then
                MsgBox "¡Acertaste 5 de 6 números! Tu boleta " & numeroColumnaA & " tiene 5 números correctos y la balota.", vbInformation
                encontrado = True
                Exit For
            End If
        Next i
    End If
    
    ' Si no está en ninguna lista
    If Not encontrado Then
        MsgBox "Lo siento, no ganaste esta vez.", vbInformation
    End If
End Sub
```

### Modulo3
```
Option Explicit

Public boletaEscogidaGlobal As String
```

## Capturas del programa
### Formulario 1
#### Formulario en el programador 
![image](https://github.com/user-attachments/assets/13a44a52-0e38-4dbe-95f1-4dbde653a61b)

#### Formulario desplegado en excel 
![image](https://github.com/user-attachments/assets/6738ecc3-5f51-4c21-bb5c-b126c433d2df)

### Ingresar boleta
![image](https://github.com/user-attachments/assets/2fbe5c65-1d0d-4d2c-b4e3-508b12cd59ee)

![image](https://github.com/user-attachments/assets/7d15c8bb-b2f5-436f-9ffe-b4efc7b7b947)

![image](https://github.com/user-attachments/assets/0ff352a2-7692-4383-a13b-a851608b9842)

![image](https://github.com/user-attachments/assets/3bb95d2e-80dd-4c63-bcb2-051cbc8be37d)

![image](https://github.com/user-attachments/assets/b3bcd44c-88d2-4fd8-9c16-a90f3a9ae697)

![image](https://github.com/user-attachments/assets/ad47e113-726e-44c0-ac63-f4986dbd2256)

#### Escoger boleta con la que se jugara
![image](https://github.com/user-attachments/assets/1109d69c-9d5c-443f-8147-584056a75eb6)

#### Jugar
![image](https://github.com/user-attachments/assets/59f0c04e-c8f8-4276-9664-fa26bd443d3e)

![image](https://github.com/user-attachments/assets/324557c0-ef75-4e1a-9fa5-d1ed07d0d9de)

![image](https://github.com/user-attachments/assets/95a565a0-f2e3-4a5b-a476-d170d8779ac7)

![image](https://github.com/user-attachments/assets/97e66256-1e31-44cf-be14-25aa9d8a6ff8)

#### Salir
![image](https://github.com/user-attachments/assets/ad716f19-9d0d-45ef-b716-e1b77d581822)

![image](https://github.com/user-attachments/assets/aa59dad0-0d38-4465-a205-ed8fd7c271f1)

### Formulario 2
#### Formulario en el programador
![image](https://github.com/user-attachments/assets/7b680e8f-bc65-449e-91e0-c02996d7da46)

### Formulario desplegado en excel 
![image](https://github.com/user-attachments/assets/f516338f-ad9c-450f-859f-39aa720ed0da)

### Obtener ganador
![image](https://github.com/user-attachments/assets/ecd5ec02-52db-4667-9d81-7a52c0268eb3)

![image](https://github.com/user-attachments/assets/b0dcb2cc-0aeb-43b0-b6a6-0b0993c8ab66)

### Verificar ganadores
![image](https://github.com/user-attachments/assets/c3874857-3dd1-4059-99fb-a4e9e680c7ca)

![image](https://github.com/user-attachments/assets/e6016e0f-1e00-4f44-845b-9e0196a46f54)

![image](https://github.com/user-attachments/assets/29b28e86-352d-4436-8e1d-58b09a91da87)

![image](https://github.com/user-attachments/assets/dfca78ad-86b0-441c-8f11-28fb462510c5)

![image](https://github.com/user-attachments/assets/10709bb8-f25d-4dd1-b0fc-bce8604d2ed4)

![image](https://github.com/user-attachments/assets/2d41214a-6af4-4785-b56d-1357fa30d6a9)

![image](https://github.com/user-attachments/assets/257fa478-fdd9-4acc-b307-bf34224b134a)

![image](https://github.com/user-attachments/assets/245288da-9c28-4cc9-a4b9-826b28971a0f)

### Regresar
![image](https://github.com/user-attachments/assets/8cfa42c6-0ce5-42e9-b082-0b12cab39e59)

![image](https://github.com/user-attachments/assets/53b94888-76cf-4410-8465-a1615019db3f)
