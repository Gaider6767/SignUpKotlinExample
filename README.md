Building a Registration Window in Kotlin with Jetpack Compose
Creating a modern, clean registration window is one of the most common tasks in Android development. By leveraging Kotlin and Jetpack Compose, you can build a responsive, secure, and beautiful UI with minimal boilerplate code.

Here is a step-by-step breakdown of how a standard registration screen is structured.

  1. Managing the State
Before building the UI, we need to track what the user types. In Compose, we use mutableStateOf to handle the inputs for the username, email, password, and confirm password fields.

  2. Crafting the UI Components
A standard registration form consists of:

OutlinedTextFields: For user input, equipped with clear labels and icons.

VisualTransformation: To hide the password characters for security.

Button: To trigger the registration logic.

EXAMPLE 1

``` kotlin
LOGIN
package com.example.signuplogininv1

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class LoginIn : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        // Я получаю email который был передан с экрана регистрации
        val savedEmail = intent.getStringExtra("email") ?: ""

        // Я получаю пароль который был передан с экрана регистрации
        val savedPassword = intent.getStringExtra("password") ?: ""

        setContent {
            LoginScreen(savedEmail, savedPassword)
        }
    }

    @Composable
    fun LoginScreen(savedEmail: String, savedPassword: String) {
        // Я сделал email изменяемым чтобы его можно было автоматически подставить и потом изменить вручную
        var email by remember { mutableStateOf(savedEmail) }

        // Я сделал пароль изменяемым чтобы его можно было автоматически подставить и потом изменить вручную
        var password by remember { mutableStateOf(savedPassword) }

        // Я добавил переменную для показа и скрытия пароля
        var passwordVisible by remember { mutableStateOf(false) }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(horizontal = 24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(90.dp))

                Text(
                    text = "Добро пожаловать!",
                    fontSize = 24.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color(0xFF3A3A3A)
                )

                Spacer(modifier = Modifier.height(8.dp))

                Text(
                    text = "Введите свой адрес электронной почты\nи пароль, чтобы продолжить",
                    fontSize = 14.sp,
                    color = Color(0xFFA7A7A7),
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(96.dp))

                Text(
                    text = "Email",
                    fontSize = 14.sp,
                    color = Color(0xFF595959),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = email,
                    onValueChange = { email = it },
                    modifier = Modifier.fillMaxWidth(),
                    placeholder = {
                        Text("**********@gmail.com", color = Color(0xFFCFCFCF))
                    },
                    shape = RoundedCornerShape(8.dp),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedBorderColor = Color(0xFFA7A7A7),
                        unfocusedBorderColor = Color(0xFFA7A7A7)
                    )
                )

                Spacer(modifier = Modifier.height(24.dp))

                Text(
                    text = "Пароль",
                    fontSize = 14.sp,
                    color = Color(0xFF595959),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    modifier = Modifier.fillMaxWidth(),
                    placeholder = {
                        Text("**********", color = Color(0xFFCFCFCF))
                    },
                    shape = RoundedCornerShape(8.dp),

                    // Я сделал условие чтобы пароль можно было показывать или скрывать
                    visualTransformation =
                        if (passwordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),

                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedBorderColor = Color(0xFFA7A7A7),
                        unfocusedBorderColor = Color(0xFFA7A7A7)
                    ),

                    trailingIcon = {
                        IconButton(onClick = {
                            // Я меняю значение переменной чтобы переключать видимость пароля
                            passwordVisible = !passwordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (passwordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (passwordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.Gray
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(180.dp))

                Button(
                    onClick = {
                        // Я добавил проверку email на пустое поле
                        if (email.isBlank()) {
                            Toast.makeText(this@LoginIn, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы email содержал знак @
                        else if (!email.contains("@")) {
                            Toast.makeText(this@LoginIn, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку пароля на пустое поле
                        else if (password.isBlank()) {
                            Toast.makeText(this@LoginIn, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я вывожу сообщение если вход прошёл успешно
                        else {
                            Toast.makeText(this@LoginIn, "Вход выполнен успешно", Toast.LENGTH_SHORT).show()
                        }
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(46.dp),
                    shape = RoundedCornerShape(8.dp),
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFFA7A7A7)
                    )
                ) {
                    Text(
                        text = "Войти",
                        fontSize = 16.sp,
                        fontWeight = FontWeight.Bold
                    )
                }

                Spacer(modifier = Modifier.height(24.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text(
                        text = "Нет аккаунта? ",
                        fontSize = 14.sp,
                        color = Color(0xFFA7A7A7)
                    )

                    Text(
                        text = "Регистрация",
                        fontSize = 14.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color(0xFF0560FA),
                        modifier = Modifier.clickable {
                            // Я сделал переход обратно на экран регистрации
                            val intent = Intent(this@LoginIn, SignUp::class.java)
                            startActivity(intent)
                        }
                    )
                }
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun LoginScreenPreview() {
        LoginScreen("", "")
    }
}
```
Creating a Registration Screen in Kotlin using XML and ViewModel
While Jetpack Compose is the future of Android, the traditional XML View system combined with View Binding and a ViewModel remains a robust, industry-standard way to build a registration window. This approach ensures a clean separation of concerns between your UI and your business logic.

1. The Architecture (MVVM)
To make the registration window scalable and testable, we divide the responsibility:

XML Layout: Defines the visual structure (EditTexts, Buttons).

Activity/Fragment: Handles UI interactions and observes data.

ViewModel: Validates inputs (e.g., checking if the email is valid or if passwords match) and communicates with the backend.

Step-by-Step Implementation
The XML Layout (activity_register.xml)
We use TextInputLayout from the Material Components library to get those smooth floating labels and built-in error placeholders.


EXAMPLE 1

``` Kotlin
SIGNUP
package com.example.signuplogininv1

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Checkbox
import androidx.compose.material3.CheckboxDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class SignUp : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            RegistrationScreen()
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun RegistrationScreen() {
        var login by remember { mutableStateOf("") }
        var email by remember { mutableStateOf("") }
        var password by remember { mutableStateOf("") }
        var confirmPassword by remember { mutableStateOf("") }
        var isPrivacyPolicyChecked by remember { mutableStateOf(false) }

        var passwordVisible by remember { mutableStateOf(false) }
        var confirmPasswordVisible by remember { mutableStateOf(false) }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(horizontal = 24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(60.dp))

                Text(
                    text = "Создайте аккаунт",
                    fontSize = 24.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color(0xFF3A3A3A)
                )

                Spacer(modifier = Modifier.height(8.dp))

                Text(
                    text = "Завершите процесс регистрации,\nчтобы приступить к работе",
                    fontSize = 14.sp,
                    color = Color(0xFFA7A7A7),
                    textAlign = TextAlign.Center,
                    modifier = Modifier.padding(horizontal = 20.dp)
                )

                Spacer(modifier = Modifier.height(32.dp))

                Text(
                    text = "Логин",
                    fontSize = 14.sp,
                    color = Color(0xFF595959),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = login,
                    onValueChange = { login = it },
                    modifier = Modifier.fillMaxWidth(),
                    placeholder = { Text("Иванов Иван", color = Color(0xFFCFCFCF)) },
                    shape = RoundedCornerShape(8.dp),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedBorderColor = Color(0xFFA7A7A7),
                        unfocusedBorderColor = Color(0xFFA7A7A7)
                    )
                )

                Spacer(modifier = Modifier.height(24.dp))

                Text(
                    text = "Email",
                    fontSize = 14.sp,
                    color = Color(0xFF595959),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = email,
                    onValueChange = { email = it },
                    modifier = Modifier.fillMaxWidth(),
                    placeholder = { Text("**********@gmail.com", color = Color(0xFFCFCFCF)) },
                    shape = RoundedCornerShape(8.dp),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedBorderColor = Color(0xFFA7A7A7),
                        unfocusedBorderColor = Color(0xFFA7A7A7)
                    )
                )

                Spacer(modifier = Modifier.height(24.dp))

                Text(
                    text = "Пароль",
                    fontSize = 14.sp,
                    color = Color(0xFF595959),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    modifier = Modifier.fillMaxWidth(),
                    placeholder = { Text("**********", color = Color(0xFFCFCFCF)) },
                    shape = RoundedCornerShape(8.dp),
                    visualTransformation =
                        if (passwordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedBorderColor = Color(0xFFA7A7A7),
                        unfocusedBorderColor = Color(0xFFA7A7A7)
                    ),
                    trailingIcon = {
                        IconButton(onClick = { passwordVisible = !passwordVisible }) {
                            Icon(
                                imageVector =
                                    if (passwordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (passwordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.Gray
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(24.dp))

                Text(
                    text = "Повторите пароль",
                    fontSize = 14.sp,
                    color = Color(0xFF595959),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = confirmPassword,
                    onValueChange = { confirmPassword = it },
                    modifier = Modifier.fillMaxWidth(),
                    placeholder = { Text("**********", color = Color(0xFFCFCFCF)) },
                    shape = RoundedCornerShape(8.dp),
                    visualTransformation =
                        if (confirmPasswordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedBorderColor = Color(0xFFA7A7A7),
                        unfocusedBorderColor = Color(0xFFA7A7A7)
                    ),
                    trailingIcon = {
                        IconButton(onClick = { confirmPasswordVisible = !confirmPasswordVisible }) {
                            Icon(
                                imageVector =
                                    if (confirmPasswordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (confirmPasswordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.Gray
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(47.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    modifier = Modifier.fillMaxWidth()
                ) {
                    Checkbox(
                        checked = isPrivacyPolicyChecked,
                        onCheckedChange = { isPrivacyPolicyChecked = it },
                        colors = CheckboxDefaults.colors(
                            checkedColor = Color(0xFF0560FA),
                            uncheckedColor = Color(0xFFA7A7A7)
                        )
                    )

                    Text(
                        text = "Отметьте это поле, если вы соглашаетесь с нашей Политикой конфиденциальности",
                        fontSize = 12.sp,
                        color = Color(0xFF8D8A8A)
                    )
                }

                Spacer(modifier = Modifier.height(106.dp))

                Button(
                    onClick = {
                        // Я добавил проверку логина на пустое поле
                        if (login.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Логин", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины логина
                        else if (login.length < 8) {
                            Toast.makeText(this@SignUp, "Логин должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку email на пустое поле
                        else if (email.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины email
                        else if (email.length < 12) {
                            Toast.makeText(this@SignUp, "Email должен быть не менее 12 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы email содержал знак @
                        else if (!email.contains("@")) {
                            Toast.makeText(this@SignUp, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку пароля на пустое поле
                        else if (password.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины пароля
                        else if (password.length < 8) {
                            Toast.makeText(this@SignUp, "Пароль должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы пароль состоял только из цифр
                        else if (!password.all { it.isDigit() }) {
                            Toast.makeText(this@SignUp, "Пароль должен состоять только из цифр", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку поля повторения пароля
                        else if (confirmPassword.isBlank()) {
                            Toast.makeText(this@SignUp, "Повторите пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку совпадения паролей
                        else if (password != confirmPassword) {
                            Toast.makeText(this@SignUp, "Пароли не совпадают", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку согласия с политикой конфиденциальности
                        else if (!isPrivacyPolicyChecked) {
                            Toast.makeText(this@SignUp, "Согласитесь с политикой конфиденциальности", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил переход на экран входа и передал туда email и пароль
                        else {
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            intent.putExtra("email", email)
                            intent.putExtra("password", password)
                            startActivity(intent)
                        }
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(46.dp),
                    shape = RoundedCornerShape(8.dp),
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFFA7A7A7)
                    )
                ) {
                    Text(
                        text = "Зарегистрироваться",
                        fontSize = 16.sp,
                        fontWeight = FontWeight.Bold
                    )
                }

                Spacer(modifier = Modifier.height(24.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    modifier = Modifier.padding(bottom = 24.dp)
                ) {
                    Text(
                        text = "Уже есть профиль? ",
                        fontSize = 14.sp,
                        color = Color(0xFFA7A7A7)
                    )

                    Text(
                        text = "Вход",
                        fontSize = 14.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color(0xFF0560FA),
                        modifier = Modifier.clickable {
                            // Я сделал текст Вход кликабельным чтобы можно было перейти на экран авторизации
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            startActivity(intent)
                        }
                    )
                }
            }
        }
    }
}
```

Key Advantages of this Approach
Input Feedback: TextInputLayout allows you to dynamically show errors (.error = "...") directly under the input field, improving the user experience.

  Security Made Easy: Setting app:passwordToggleEnabled="true" in XML automatically adds an eye icon allowing users to toggle password visibility securely.
Input Types: Specifying android:inputType="textEmailAddress" ensures the mobile device opens a keyboard optimized for typing emails (complete with the @ symbol).

```XML 1
XML
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SignUpLoginInv1">

        <activity
            android:name=".LoginIn"
            android:exported="false" />

        <activity
            android:name=".SignUp"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```
Building a Cross-Platform Registration Window with Compose Multiplatform
With Compose Multiplatform (by JetBrains), you can write your Kotlin UI code once and run it on Android, iOS, Desktop, and Web. This version focuses on a highly responsive, polished UI that includes theme switching and advanced input validation feedback.

  1. Key Concept: State-Driven UI
In a professional production app, you don't just check if fields are empty on button click. Instead, the UI reacts instantly as the user types. We use visual hints (like changing border colors) to guide the user before they even press "Submit."


EXAMPLE 2 LOGIN

```Kotlin
LOGIN

package com.example.signuplogininv2

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class LoginIn : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        // Я получаю email который был передан с экрана регистрации
        val savedEmail = intent.getStringExtra("email") ?: ""

        // Я получаю пароль который был передан с экрана регистрации
        val savedPassword = intent.getStringExtra("password") ?: ""

        setContent {
            LoginScreen(savedEmail, savedPassword)
        }
    }

    @Composable
    fun LoginScreen(savedEmail: String, savedPassword: String) {
        // Я сделал email изменяемым чтобы он подставлялся автоматически и его можно было изменить
        var email by remember { mutableStateOf(savedEmail) }

        // Я сделал пароль изменяемым чтобы он подставлялся автоматически и его можно было изменить
        var password by remember { mutableStateOf(savedPassword) }

        // Я добавил переменную для показа и скрытия пароля
        var passwordVisible by remember { mutableStateOf(false) }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(horizontal = 24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(100.dp))

                Text(
                    text = "Привет!",
                    fontSize = 32.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color(0xFF2B2B2B)
                )

                Spacer(modifier = Modifier.height(8.dp))

                Text(
                    text = "Заполните Свои данные Или\nПродолжите Через Социальные Медиа",
                    fontSize = 16.sp,
                    color = Color(0xFF707B81),
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(44.dp))

                Text(
                    text = "Email",
                    fontSize = 16.sp,
                    color = Color(0xFF2B2B2B),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = email,
                    onValueChange = { email = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********@gmail.com", color = Color(0xFF6A6A6A))
                    },
                    shape = RoundedCornerShape(14.dp),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.Black,
                        unfocusedTextColor = Color.Black,
                        focusedContainerColor = Color(0xFFF7F7F9),
                        unfocusedContainerColor = Color(0xFFF7F7F9),
                        focusedBorderColor = Color.Transparent,
                        unfocusedBorderColor = Color.Transparent,
                        cursorColor = Color.Black
                    )
                )

                Spacer(modifier = Modifier.height(24.dp))

                Text(
                    text = "Пароль",
                    fontSize = 16.sp,
                    color = Color(0xFF2B2B2B),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("********", color = Color(0xFF6A6A6A))
                    },
                    shape = RoundedCornerShape(14.dp),

                    // Я сделал условие чтобы пароль можно было показывать или скрывать
                    visualTransformation =
                        if (passwordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),

                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.Black,
                        unfocusedTextColor = Color.Black,
                        focusedContainerColor = Color(0xFFF7F7F9),
                        unfocusedContainerColor = Color(0xFFF7F7F9),
                        focusedBorderColor = Color.Transparent,
                        unfocusedBorderColor = Color.Transparent,
                        cursorColor = Color.Black
                    ),
                    trailingIcon = {
                        IconButton(onClick = {
                            // Я меняю значение переменной чтобы переключать видимость пароля
                            passwordVisible = !passwordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (passwordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (passwordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.Gray
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(24.dp))

                Button(
                    onClick = {
                        // Я добавил проверку email на пустое поле
                        if (email.isBlank()) {
                            Toast.makeText(this@LoginIn, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы email содержал знак @
                        else if (!email.contains("@")) {
                            Toast.makeText(this@LoginIn, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку пароля на пустое поле
                        else if (password.isBlank()) {
                            Toast.makeText(this@LoginIn, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я вывожу сообщение если вход выполнен успешно
                        else {
                            Toast.makeText(this@LoginIn, "Вход выполнен успешно", Toast.LENGTH_SHORT).show()
                        }
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(54.dp),
                    shape = RoundedCornerShape(13.dp),
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFF48B2E7)
                    )
                ) {
                    Text(
                        text = "Войти",
                        fontSize = 14.sp
                    )
                }

                Spacer(modifier = Modifier.height(180.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text(
                        text = "Вы впервые? ",
                        fontSize = 16.sp,
                        color = Color(0xFF6A6A6A)
                    )

                    Text(
                        text = "Создать пользователя",
                        fontSize = 16.sp,
                        color = Color(0xFF2B2B2B),
                        modifier = Modifier.clickable {
                            // Я сделал переход обратно на экран регистрации
                            val intent = Intent(this@LoginIn, SignUp::class.java)
                            startActivity(intent)
                        }
                    )
                }
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun LoginScreenPreview() {
        LoginScreen("", "")
    }
}
```

 1. Key Concept: State-Driven UI
In a professional production app, you don't just check if fields are empty on button click. Instead, the UI reacts instantly as the user types. We use visual hints (like changing border colors) to guide the user before they even press "Submit."
EXAMPLE 2 SIGNUP

```Kotlin
SignUP
package com.example.signuplogininv2

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Checkbox
import androidx.compose.material3.CheckboxDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.text.style.TextDecoration
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class SignUp : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            RegistrationScreen()
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun RegistrationScreen() {
        var login by remember { mutableStateOf("") }
        var email by remember { mutableStateOf("") }
        var password by remember { mutableStateOf("") }
        var confirmPassword by remember { mutableStateOf("") }
        var isPrivacyPolicyChecked by remember { mutableStateOf(false) }

        var passwordVisible by remember { mutableStateOf(false) }
        var confirmPasswordVisible by remember { mutableStateOf(false) }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(horizontal = 24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(60.dp))

                Text(
                    text = "Регистрация",
                    fontSize = 32.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color(0xFF2B2B2B)
                )

                Spacer(modifier = Modifier.height(8.dp))

                Text(
                    text = "Заполните Свои данные Или\n Продолжите Через Социальные Медиа",
                    fontSize = 16.sp,
                    color = Color(0xFF707B81),
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(22.dp))

                Text(
                    text = "Логин",
                    fontSize = 16.sp,
                    color = Color(0xFF2B2B2B),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = login,
                    onValueChange = { login = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("loginperson", color = Color(0xFF6A6A6A))
                    },
                    shape = RoundedCornerShape(14.dp),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.Black,
                        unfocusedTextColor = Color.Black,
                        focusedBorderColor = Color(0xFF48B2E7),
                        unfocusedBorderColor = Color(0xFF48B2E7),
                        cursorColor = Color.Black
                    )
                )

                Spacer(modifier = Modifier.height(12.dp))

                Text(
                    text = "Email",
                    fontSize = 16.sp,
                    color = Color(0xFF2B2B2B),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = email,
                    onValueChange = { email = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********@gmail.com", color = Color(0xFF6A6A6A))
                    },
                    shape = RoundedCornerShape(14.dp),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.Black,
                        unfocusedTextColor = Color.Black,
                        focusedBorderColor = Color(0xFF48B2E7),
                        unfocusedBorderColor = Color(0xFF48B2E7),
                        cursorColor = Color.Black
                    )
                )

                Spacer(modifier = Modifier.height(12.dp))

                Text(
                    text = "Пароль",
                    fontSize = 16.sp,
                    color = Color(0xFF2B2B2B),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********", color = Color(0xFF6A6A6A))
                    },
                    shape = RoundedCornerShape(14.dp),
                    visualTransformation =
                        if (passwordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.Black,
                        unfocusedTextColor = Color.Black,
                        focusedBorderColor = Color(0xFF48B2E7),
                        unfocusedBorderColor = Color(0xFF48B2E7),
                        cursorColor = Color.Black
                    ),
                    trailingIcon = {
                        IconButton(onClick = {
                            passwordVisible = !passwordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (passwordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (passwordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.Gray
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(12.dp))

                Text(
                    text = "Повторите пароль",
                    fontSize = 16.sp,
                    color = Color(0xFF2B2B2B),
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = confirmPassword,
                    onValueChange = { confirmPassword = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********", color = Color(0xFF6A6A6A))
                    },
                    shape = RoundedCornerShape(14.dp),
                    visualTransformation =
                        if (confirmPasswordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.Black,
                        unfocusedTextColor = Color.Black,
                        focusedBorderColor = Color(0xFF48B2E7),
                        unfocusedBorderColor = Color(0xFF48B2E7),
                        cursorColor = Color.Black
                    ),
                    trailingIcon = {
                        IconButton(onClick = {
                            confirmPasswordVisible = !confirmPasswordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (confirmPasswordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (confirmPasswordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.Gray
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(12.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    modifier = Modifier.fillMaxWidth()
                ) {
                    Checkbox(
                        checked = isPrivacyPolicyChecked,
                        onCheckedChange = { isPrivacyPolicyChecked = it },
                        colors = CheckboxDefaults.colors(
                            checkedColor = Color(0xFF0560FA),
                            uncheckedColor = Color(0xFF6A6A6A)
                        )
                    )

                    Text(
                        text = "Даю согласие на обработку\nперсональных данных",
                        fontSize = 16.sp,
                        color = Color(0xFF6A6A6A),
                        textDecoration = TextDecoration.Underline
                    )
                }

                Spacer(modifier = Modifier.height(106.dp))

                Button(
                    onClick = {
                        // Я добавил проверку логина на пустое поле
                        if (login.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Логин", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины логина
                        else if (login.length < 8) {
                            Toast.makeText(this@SignUp, "Логин должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку email на пустое поле
                        else if (email.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины email
                        else if (email.length < 12) {
                            Toast.makeText(this@SignUp, "Email должен быть не менее 12 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы email содержал знак @
                        else if (!email.contains("@")) {
                            Toast.makeText(this@SignUp, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку пароля на пустое поле
                        else if (password.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины пароля
                        else if (password.length < 8) {
                            Toast.makeText(this@SignUp, "Пароль должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы пароль состоял только из цифр
                        else if (!password.all { it.isDigit() }) {
                            Toast.makeText(this@SignUp, "Пароль должен состоять только из цифр", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку поля повторения пароля
                        else if (confirmPassword.isBlank()) {
                            Toast.makeText(this@SignUp, "Повторите пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку совпадения паролей
                        else if (password != confirmPassword) {
                            Toast.makeText(this@SignUp, "Пароли не совпадают", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку согласия на обработку персональных данных
                        else if (!isPrivacyPolicyChecked) {
                            Toast.makeText(this@SignUp, "Дайте согласие на обработку персональных данных", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил переход на экран входа и передал туда email и пароль
                        else {
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            intent.putExtra("email", email)
                            intent.putExtra("password", password)
                            startActivity(intent)
                        }
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(46.dp),
                    shape = RoundedCornerShape(13.dp),
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFF48B2E7)
                    )
                ) {
                    Text(
                        text = "Зарегистрироваться",
                        fontSize = 14.sp
                    )
                }

                Spacer(modifier = Modifier.height(24.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    modifier = Modifier.padding(bottom = 24.dp)
                ) {
                    Text(
                        text = "Есть аккаунт? ",
                        fontSize = 16.sp,
                        color = Color(0xFF6A6A6A)
                    )

                    Text(
                        text = "Войти",
                        fontSize = 16.sp,
                        color = Color(0xFF2B2B2B),
                        modifier = Modifier.clickable {
                            // Я сделал текст Войти кликабельным чтобы можно было перейти на экран авторизации
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            startActivity(intent)
                        }
                    )
                }
            }
        }
    }
}

```
In a professional production app, you don't just check if fields are empty on button click. Instead, the UI reacts instantly as the user types. We use visual hints (like changing border colors) to guide the user before they even press "Submit."

EXAMPLE 2 XML

``` Kotlin
XML
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SignUpLoginInv2">

        <activity
            android:name=".LoginIn"
            android:exported="false" />

        <activity
            android:name=".SignUp"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```
Creating a Web Registration Window in Kotlin using Ktor and HTML DSL
Kotlin isn't just for mobile and desktop apps; it's a powerful tool for server-side web development. Using Ktor (JetBrains' asynchronous web framework) and the Kotlinx.html DSL, you can generate HTML forms dynamically on the server side with total type safety.

1. The Full-Stack Kotlin Concept
Instead of writing HTML files manually, Kotlin allows you to write HTML structure directly inside your backend routing using pure Kotlin code. This makes sharing data classes (like user registration models) between your frontend template and your database completely seamless.

EXAMPLE 3 LOG IN 

```Kotlin
package com.example.signuplogininv3

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.HorizontalDivider
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class LoginIn : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        // Я получаю email который был передан с экрана регистрации
        val savedEmail = intent.getStringExtra("email") ?: ""

        // Я получаю пароль который был передан с экрана регистрации
        val savedPassword = intent.getStringExtra("password") ?: ""

        setContent {
            LoginScreen(savedEmail, savedPassword)
        }
    }

    @Composable
    fun LoginScreen(savedEmail: String, savedPassword: String) {
        // Я сделал email изменяемым чтобы он автоматически подставлялся и его можно было изменить вручную
        var email by remember { mutableStateOf(savedEmail) }

        // Я сделал пароль изменяемым чтобы он автоматически подставлялся и его можно было изменить вручную
        var password by remember { mutableStateOf(savedPassword) }

        // Я добавил переменную для показа и скрытия пароля
        var passwordVisible by remember { mutableStateOf(false) }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color(0xFF010827))
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(horizontal = 24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(140.dp))

                Text(
                    text = "THIRDY BIZ",
                    fontSize = 41.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color.White
                )

                Text(
                    text = "Digital Calling Card",
                    fontSize = 20.sp,
                    color = Color.White,
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(24.dp))

                HorizontalDivider(
                    modifier = Modifier.fillMaxWidth(),
                    thickness = 1.dp,
                    color = Color(0xFF928787)
                )

                Spacer(modifier = Modifier.height(34.dp))

                Text(
                    text = "ВХОД",
                    fontSize = 20.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(44.dp))

                Text(
                    text = "Email",
                    fontSize = 15.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = email,
                    onValueChange = { email = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********@gmail.com", color = Color.White)
                    },
                    singleLine = true,
                    shape = RoundedCornerShape(8.dp),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color(0xFF928787),
                        unfocusedBorderColor = Color(0xFF928787),
                        focusedContainerColor = Color.Transparent,
                        unfocusedContainerColor = Color.Transparent,
                        cursorColor = Color.White
                    )
                )

                Spacer(modifier = Modifier.height(26.dp))

                Text(
                    text = "Пароль",
                    fontSize = 15.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp)
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********", color = Color.White)
                    },
                    singleLine = true,
                    shape = RoundedCornerShape(8.dp),

                    // Я сделал условие чтобы пароль можно было показывать или скрывать
                    visualTransformation =
                        if (passwordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),

                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color(0xFF928787),
                        unfocusedBorderColor = Color(0xFF928787),
                        focusedContainerColor = Color.Transparent,
                        unfocusedContainerColor = Color.Transparent,
                        cursorColor = Color.White
                    ),
                    trailingIcon = {
                        IconButton(onClick = {
                            // Я меняю значение переменной чтобы переключать видимость пароля
                            passwordVisible = !passwordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (passwordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (passwordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.White
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(70.dp))

                Button(
                    onClick = {
                        // Я добавил проверку email на пустое поле
                        if (email.isBlank()) {
                            Toast.makeText(this@LoginIn, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы email содержал знак @
                        else if (!email.contains("@")) {
                            Toast.makeText(this@LoginIn, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку пароля на пустое поле
                        else if (password.isBlank()) {
                            Toast.makeText(this@LoginIn, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я вывожу сообщение если вход выполнен успешно
                        else {
                            Toast.makeText(this@LoginIn, "Вход выполнен успешно", Toast.LENGTH_SHORT).show()
                        }
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(46.dp),
                    shape = RoundedCornerShape(8.dp),
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFF9B0DD9)
                    )
                ) {
                    Text(
                        text = "ВОЙТИ",
                        fontSize = 15.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color.White
                    )
                }

                Spacer(modifier = Modifier.height(24.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text(
                        text = "Нет аккаунта? ",
                        fontSize = 16.sp,
                        color = Color(0xFF6A6A6A)
                    )

                    Text(
                        text = "Регистрация",
                        fontSize = 16.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color.White,
                        modifier = Modifier.clickable {
                            // Я сделал переход обратно на экран регистрации
                            val intent = Intent(this@LoginIn, SignUp::class.java)
                            startActivity(intent)
                        }
                    )
                }
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun LoginScreenPreview() {
        LoginScreen("", "")
    }
}
```
 Highlights of Web-Based Kotlin UI
  Type-Safe HTML (DSL): Notice how div, form, and textInput are regular Kotlin blocks. If you misspell an HTML tag, your project won't compile, saving you from standard web typos.
Tailwind CSS Integration: By including the Tailwind library in the <head>, we can style elements explicitly using clean utility strings like bg-gray-100 (light background) and shadow-md (drop shadow effect) right within Kotlin.
Native Form Handling: The Ktor backend processes inbound data asynchronously using call.receiveParameters(), seamlessly binding HTTP parameters to backend variables.

EXAMPLE 3 SignUP

```Kotlin
SignUP
package com.example.signuplogininv3

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class SignUp : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            RegistrationScreen()
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun RegistrationScreen() {
        var login by remember { mutableStateOf("") }
        var email by remember { mutableStateOf("") }
        var password by remember { mutableStateOf("") }
        var confirmPassword by remember { mutableStateOf("") }

        var passwordVisible by remember { mutableStateOf(false) }
        var confirmPasswordVisible by remember { mutableStateOf(false) }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color(0xFF010827))
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(horizontal = 24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(50.dp))

                Text(
                    text = "THIRDY BIZ",
                    fontSize = 41.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color.White
                )

                Text(
                    text = "Digital Calling Card",
                    fontSize = 20.sp,
                    color = Color.White,
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(30.dp))

                Text(
                    text = "НАЧНЕМ",
                    fontSize = 20.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    textAlign = TextAlign.Center
                )

                Spacer(modifier = Modifier.height(4.dp))

                Text(
                    text = "Логин",
                    fontSize = 15.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp, top = 36.dp)
                )

                OutlinedTextField(
                    value = login,
                    onValueChange = { login = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 11.dp),
                    placeholder = {
                        Text("Ivanov Ivan", color = Color.White)
                    },
                    singleLine = true,
                    shape = RoundedCornerShape(8.dp),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color(0xFF928787),
                        unfocusedBorderColor = Color(0xFF928787),
                        focusedContainerColor = Color.Transparent,
                        unfocusedContainerColor = Color.Transparent,
                        cursorColor = Color.White
                    )
                )

                Spacer(modifier = Modifier.height(12.dp))

                Text(
                    text = "Email",
                    fontSize = 15.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp, top = 26.dp)
                )

                OutlinedTextField(
                    value = email,
                    onValueChange = { email = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********@gmail.com", color = Color.White)
                    },
                    singleLine = true,
                    shape = RoundedCornerShape(8.dp),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color(0xFF928787),
                        unfocusedBorderColor = Color(0xFF928787),
                        focusedContainerColor = Color.Transparent,
                        unfocusedContainerColor = Color.Transparent,
                        cursorColor = Color.White
                    )
                )

                Spacer(modifier = Modifier.height(12.dp))

                Text(
                    text = "Пароль",
                    fontSize = 15.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp, top = 26.dp)
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 11.dp),
                    placeholder = {
                        Text("**********", color = Color.White)
                    },
                    singleLine = true,
                    shape = RoundedCornerShape(8.dp),
                    visualTransformation =
                        if (passwordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color(0xFF928787),
                        unfocusedBorderColor = Color(0xFF928787),
                        focusedContainerColor = Color.Transparent,
                        unfocusedContainerColor = Color.Transparent,
                        cursorColor = Color.White
                    ),
                    trailingIcon = {
                        IconButton(onClick = {
                            passwordVisible = !passwordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (passwordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (passwordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.White
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(12.dp))

                Text(
                    text = "Повторите пароль",
                    fontSize = 15.sp,
                    color = Color.White,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .align(Alignment.Start)
                        .padding(start = 4.dp, top = 26.dp)
                )

                OutlinedTextField(
                    value = confirmPassword,
                    onValueChange = { confirmPassword = it },
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(top = 12.dp),
                    placeholder = {
                        Text("**********", color = Color.White)
                    },
                    singleLine = true,
                    shape = RoundedCornerShape(8.dp),
                    visualTransformation =
                        if (confirmPasswordVisible) VisualTransformation.None
                        else PasswordVisualTransformation(),
                    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color(0xFF928787),
                        unfocusedBorderColor = Color(0xFF928787),
                        focusedContainerColor = Color.Transparent,
                        unfocusedContainerColor = Color.Transparent,
                        cursorColor = Color.White
                    ),
                    trailingIcon = {
                        IconButton(onClick = {
                            confirmPasswordVisible = !confirmPasswordVisible
                        }) {
                            Icon(
                                imageVector =
                                    if (confirmPasswordVisible) Icons.Default.Visibility
                                    else Icons.Default.VisibilityOff,
                                contentDescription =
                                    if (confirmPasswordVisible) "Скрыть пароль"
                                    else "Показать пароль",
                                tint = Color.White
                            )
                        }
                    }
                )

                Spacer(modifier = Modifier.height(36.dp))

                Button(
                    onClick = {
                        // Я добавил проверку логина на пустое поле
                        if (login.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Логин", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины логина
                        else if (login.length < 8) {
                            Toast.makeText(this@SignUp, "Логин должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку email на пустое поле
                        else if (email.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины email
                        else if (email.length < 12) {
                            Toast.makeText(this@SignUp, "Email должен быть не менее 12 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы email содержал знак @
                        else if (!email.contains("@")) {
                            Toast.makeText(this@SignUp, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку пароля на пустое поле
                        else if (password.isBlank()) {
                            Toast.makeText(this@SignUp, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку длины пароля
                        else if (password.length < 8) {
                            Toast.makeText(this@SignUp, "Пароль должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку чтобы пароль состоял только из цифр
                        else if (!password.all { it.isDigit() }) {
                            Toast.makeText(this@SignUp, "Пароль должен состоять только из цифр", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку поля повторения пароля
                        else if (confirmPassword.isBlank()) {
                            Toast.makeText(this@SignUp, "Повторите пароль", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил проверку совпадения паролей
                        else if (password != confirmPassword) {
                            Toast.makeText(this@SignUp, "Пароли не совпадают", Toast.LENGTH_SHORT).show()
                        }

                        // Я добавил переход на экран входа и передал туда email и пароль
                        else {
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            intent.putExtra("email", email)
                            intent.putExtra("password", password)
                            startActivity(intent)
                        }
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(46.dp),
                    shape = RoundedCornerShape(8.dp),
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFF9B0DD9)
                    )
                ) {
                    Text(
                        text = "Зарегистрироваться",
                        fontSize = 15.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color.White
                    )
                }

                Spacer(modifier = Modifier.height(24.dp))

                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    modifier = Modifier.padding(bottom = 24.dp)
                ) {
                    Text(
                        text = "Есть аккаунт? ",
                        fontSize = 16.sp,
                        color = Color(0xFF6A6A6A)
                    )

                    Text(
                        text = "Войти",
                        fontSize = 16.sp,
                        color = Color.White,
                        modifier = Modifier.clickable {
                            // Я сделал текст Войти кликабельным чтобы можно было перейти на экран авторизации
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            startActivity(intent)
                        }
                    )
                }
            }
        }
    }
}
```
EXAMPLE 3 XML

```Kotlin
XML
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SignUpLoginInv3">

        <activity
            android:name=".LoginIn"
            android:exported="false" />

        <activity
            android:name=".SignUp"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
```
Shared Architecture: Registration Window via Kotlin Multiplatform (KMP) & MVI
When building a commercial app for both iOS and Android, you don't want to rewrite registration validation logic twice. By separating the State and Intent into a shared Kotlin module, you can guarantee identical login flows across all user devices.

1. The MVI Flow Architecture
State: A single immutable data class representing exactly what the screen shows (loading, error, fields).
Intent: User actions (e.g., TypeEmail, ClickRegister).
Reducer: A pure Kotlin function that takes the old state, handles the intent, and outputs a new state.


EXAMPLE 4 (XML + SignUP + LogIN)

```Kotlin
LogIN

package com.example.signuplogininv4

import android.content.Intent
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.wrapContentSize
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Card
import androidx.compose.material3.HorizontalDivider
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.geometry.Offset
import androidx.compose.ui.graphics.Brush
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.vector.ImageVector
import androidx.compose.ui.res.vectorResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class LoginIn : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        // Я получаю email который был передан с экрана регистрации
        val savedEmail = intent.getStringExtra("email") ?: ""

        // Я получаю пароль который был передан с экрана регистрации
        val savedPassword = intent.getStringExtra("password") ?: ""

        setContent {
            LoginScreen(savedEmail, savedPassword)
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun LoginScreenPreview() {
        LoginScreen("", "")
    }

    @Composable
    fun LoginScreen(savedEmail: String, savedPassword: String) {
        // Я сделал email изменяемым чтобы он автоматически подставлялся и его можно было изменить вручную
        var email by remember { mutableStateOf(savedEmail) }

        // Я сделал пароль изменяемым чтобы он автоматически подставлялся и его можно было изменить вручную
        var password by remember { mutableStateOf(savedPassword) }

        // Я добавил переменную для показа и скрытия пароля
        var passwordVisible by remember { mutableStateOf(false) }

        val gradient = Brush.linearGradient(
            0.0f to Color(0xFF2473E6),
            500.0f to Color(0xFF1CE0DA),
            start = Offset.Zero,
            end = Offset.Infinite
        )

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(gradient)
        ) {
            Spacer(modifier = Modifier.height(170.dp))

            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.Center,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Icon(
                    imageVector = ImageVector.vectorResource(R.drawable.vector),
                    contentDescription = null,
                    tint = Color.White
                )

                Text(
                    text = "Logoipsum",
                    fontSize = 20.sp,
                    fontWeight = FontWeight.Bold,
                    textAlign = TextAlign.Center,
                    color = Color.White,
                    modifier = Modifier.padding(start = 7.dp)
                )
            }

            Spacer(modifier = Modifier.height(45.dp))

            Card(
                modifier = Modifier
                    .wrapContentSize()
                    .padding(start = 16.dp, end = 16.dp),
                shape = RoundedCornerShape(12.dp)
            ) {
                Column(
                    modifier = Modifier
                        .fillMaxWidth()
                        .background(Color.White)
                        .padding(bottom = 24.dp),
                    horizontalAlignment = Alignment.CenterHorizontally
                ) {
                    Spacer(modifier = Modifier.height(32.dp))

                    Text(
                        text = "Вход",
                        fontSize = 41.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color(0xFF111827)
                    )

                    Spacer(modifier = Modifier.height(12.dp))

                    Row {
                        Text(
                            text = "Нет аккаунта?",
                            fontSize = 12.sp,
                            color = Color(0xFF6C7278),
                            textAlign = TextAlign.Center
                        )

                        Text(
                            text = "Регистрация",
                            fontSize = 12.sp,
                            color = Color(0xFF4D81E7),
                            fontWeight = FontWeight.Bold,
                            textAlign = TextAlign.Center,
                            modifier = Modifier
                                .padding(start = 6.dp)
                                .clickable {
                                    // Я сделал переход обратно на экран регистрации
                                    val intent = Intent(this@LoginIn, SignUp::class.java)
                                    startActivity(intent)
                                }
                        )
                    }

                    Text(
                        text = "Email",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 24.dp)
                    )

                    OutlinedTextField(
                        value = email,
                        onValueChange = { email = it },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp),
                        placeholder = {
                            Text("primer@gmail.com", color = Color.Black)
                        },
                        singleLine = true,
                        shape = RoundedCornerShape(8.dp),
                        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        )
                    )

                    Text(
                        text = "Пароль",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 16.dp)
                    )

                    OutlinedTextField(
                        value = password,
                        onValueChange = { password = it },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp),
                        placeholder = {
                            Text("**********", color = Color.Black)
                        },
                        singleLine = true,
                        shape = RoundedCornerShape(8.dp),

                        // Я сделал условие чтобы пароль можно было показывать или скрывать
                        visualTransformation =
                            if (passwordVisible) VisualTransformation.None
                            else PasswordVisualTransformation(),

                        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        ),
                        trailingIcon = {
                            IconButton(onClick = {
                                // Я меняю значение переменной чтобы переключать видимость пароля
                                passwordVisible = !passwordVisible
                            }) {
                                Icon(
                                    imageVector =
                                        if (passwordVisible) Icons.Default.Visibility
                                        else Icons.Default.VisibilityOff,
                                    contentDescription =
                                        if (passwordVisible) "Скрыть пароль"
                                        else "Показать пароль",
                                    tint = Color.Gray
                                )
                            }
                        }
                    )

                    Spacer(modifier = Modifier.height(24.dp))

                    Button(
                        onClick = {
                            // Я добавил проверку email на пустое поле
                            if (email.isBlank()) {
                                Toast.makeText(this@LoginIn, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку чтобы email содержал знак @
                            else if (!email.contains("@")) {
                                Toast.makeText(this@LoginIn, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку пароля на пустое поле
                            else if (password.isBlank()) {
                                Toast.makeText(this@LoginIn, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                            }

                            // Я вывожу сообщение если вход выполнен успешно
                            else {
                                Toast.makeText(this@LoginIn, "Вход выполнен успешно", Toast.LENGTH_SHORT).show()
                            }
                        },
                        modifier = Modifier
                            .fillMaxWidth()
                            .height(48.dp)
                            .padding(horizontal = 24.dp),
                        shape = RoundedCornerShape(10.dp),
                        colors = ButtonDefaults.buttonColors(
                            containerColor = Color(0xFF1D61E7)
                        )
                    ) {
                        Text(
                            text = "Войти",
                            fontSize = 15.sp,
                            color = Color.White
                        )
                    }

                    Spacer(modifier = Modifier.height(24.dp))

                    HorizontalDivider(
                        modifier = Modifier.padding(horizontal = 24.dp),
                        thickness = 1.dp,
                        color = Color(0xFFE5E7EB)
                    )
                }
            }
        }
    }
}


SignUP

package com.example.signuplogininv4

import android.app.DatePickerDialog
import android.content.Context
import android.content.Intent
import android.icu.util.Calendar
import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.wrapContentSize
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.DateRange
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Card
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.geometry.Offset
import androidx.compose.ui.graphics.Brush
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.vector.ImageVector
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.res.vectorResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import java.util.Date

class SignUp : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            val context = LocalContext.current
            RegistrationScreen(context)
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun RegistrationScreenPreview() {
        val context = LocalContext.current
        RegistrationScreen(context)
    }

    @Composable
    fun RegistrationScreen(context: Context) {
        var login by remember { mutableStateOf("") }
        var email by remember { mutableStateOf("") }
        var password by remember { mutableStateOf("") }
        var confirmPassword by remember { mutableStateOf("") }

        var passwordVisible by remember { mutableStateOf(false) }
        var confirmPasswordVisible by remember { mutableStateOf(false) }

        // Я добавил переменную для даты рождения
        var dateOfBirth by remember { mutableStateOf("") }

        // Я добавил DatePickerDialog для выбора даты рождения
        val calendary = Calendar.getInstance()
        val year = calendary.get(Calendar.YEAR)
        val month = calendary.get(Calendar.MONTH)
        val day = calendary.get(Calendar.DAY_OF_MONTH)
        calendary.time = Date()

        val datePicker = DatePickerDialog(
            context,
            { _, mYear, mMonth, mDayOfMonth ->
                dateOfBirth = "$mDayOfMonth.${mMonth + 1}.$mYear"
            },
            year,
            month,
            day
        )

        val gradient = Brush.linearGradient(
            0.0f to Color(0xFF2473E6),
            500.0f to Color(0xFF1CE0DA),
            start = Offset.Zero,
            end = Offset.Infinite
        )

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(gradient)
        ) {
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(top = 50.dp, bottom = 25.dp),
                horizontalArrangement = Arrangement.Center,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Icon(
                    imageVector = ImageVector.vectorResource(R.drawable.vector),
                    contentDescription = null,
                    tint = Color.White
                )

                Text(
                    text = "Logoipsum",
                    fontSize = 20.sp,
                    fontWeight = FontWeight.Bold,
                    textAlign = TextAlign.Center,
                    color = Color.White,
                    modifier = Modifier.padding(start = 7.dp)
                )
            }

            Card(
                modifier = Modifier
                    .wrapContentSize()
                    .padding(start = 16.dp, end = 16.dp, bottom = 71.dp),
                shape = RoundedCornerShape(12.dp)
            ) {
                Column(
                    modifier = Modifier
                        .fillMaxSize()
                        .background(Color.White),
                    horizontalAlignment = Alignment.CenterHorizontally
                ) {
                    Spacer(modifier = Modifier.height(24.dp))

                    Text(
                        text = "Регистрация",
                        fontSize = 41.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color(0xFF111827)
                    )

                    Spacer(modifier = Modifier.height(12.dp))

                    Row {
                        Text(
                            text = "Уже есть аккаунт?",
                            fontSize = 12.sp,
                            color = Color(0xFF6C7278),
                            textAlign = TextAlign.Center
                        )

                        Text(
                            text = "Вход",
                            fontSize = 12.sp,
                            color = Color(0xFF4D81E7),
                            fontWeight = FontWeight.Bold,
                            textAlign = TextAlign.Center,
                            modifier = Modifier
                                .padding(start = 6.dp)
                                .clickable {
                                    // Я сделал переход на экран входа по тексту Вход
                                    val intent = Intent(this@SignUp, LoginIn::class.java)
                                    startActivity(intent)
                                }
                        )
                    }

                    Spacer(modifier = Modifier.height(4.dp))

                    Text(
                        text = "Логин",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 24.dp)
                    )

                    OutlinedTextField(
                        value = login,
                        onValueChange = { login = it },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp),
                        placeholder = {
                            Text("loginperson", color = Color.Black)
                        },
                        singleLine = true,
                        shape = RoundedCornerShape(8.dp),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        )
                    )

                    Text(
                        text = "Email",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 16.dp)
                    )

                    OutlinedTextField(
                        value = email,
                        onValueChange = { email = it },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp),
                        placeholder = {
                            Text("primer@gmail.com", color = Color.Black)
                        },
                        singleLine = true,
                        shape = RoundedCornerShape(8.dp),
                        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        )
                    )

                    Text(
                        text = "Дата рождения",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 16.dp)
                    )

                    OutlinedTextField(
                        value = dateOfBirth,
                        onValueChange = { },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp)
                            .clickable {
                                // Я сделал открытие календаря при нажатии на поле даты
                                datePicker.show()
                            },
                        placeholder = {
                            Text("18.03.2003", color = Color.Black)
                        },
                        singleLine = true,
                        readOnly = true,
                        shape = RoundedCornerShape(8.dp),
                        trailingIcon = {
                            IconButton(onClick = {
                                // Я также сделал открытие календаря по иконке
                                datePicker.show()
                            }) {
                                Icon(
                                    imageVector = Icons.Default.DateRange,
                                    contentDescription = "Выбрать дату",
                                    tint = Color.Gray
                                )
                            }
                        },
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        )
                    )

                    Text(
                        text = "Пароль",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 16.dp)
                    )

                    OutlinedTextField(
                        value = password,
                        onValueChange = { password = it },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp),
                        placeholder = {
                            Text("**********", color = Color.Black)
                        },
                        singleLine = true,
                        shape = RoundedCornerShape(8.dp),
                        visualTransformation =
                            if (passwordVisible) VisualTransformation.None
                            else PasswordVisualTransformation(),
                        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        ),
                        trailingIcon = {
                            IconButton(onClick = {
                                // Я сделал переключение видимости пароля
                                passwordVisible = !passwordVisible
                            }) {
                                Icon(
                                    imageVector =
                                        if (passwordVisible) Icons.Default.Visibility
                                        else Icons.Default.VisibilityOff,
                                    contentDescription =
                                        if (passwordVisible) "Скрыть пароль"
                                        else "Показать пароль",
                                    tint = Color.Gray
                                )
                            }
                        }
                    )

                    Text(
                        text = "Повторите пароль",
                        fontSize = 12.sp,
                        color = Color(0xFF6C7278),
                        modifier = Modifier
                            .align(Alignment.Start)
                            .padding(start = 24.dp, top = 16.dp)
                    )

                    OutlinedTextField(
                        value = confirmPassword,
                        onValueChange = { confirmPassword = it },
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(start = 24.dp, end = 24.dp, top = 2.dp),
                        placeholder = {
                            Text("**********", color = Color.Black)
                        },
                        singleLine = true,
                        shape = RoundedCornerShape(8.dp),
                        visualTransformation =
                            if (confirmPasswordVisible) VisualTransformation.None
                            else PasswordVisualTransformation(),
                        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.Black,
                            unfocusedTextColor = Color.Black,
                            focusedBorderColor = Color(0xFFE5E7EB),
                            unfocusedBorderColor = Color(0xFFE5E7EB),
                            cursorColor = Color.Black
                        ),
                        trailingIcon = {
                            IconButton(onClick = {
                                // Я сделал переключение видимости повторного пароля
                                confirmPasswordVisible = !confirmPasswordVisible
                            }) {
                                Icon(
                                    imageVector =
                                        if (confirmPasswordVisible) Icons.Default.Visibility
                                        else Icons.Default.VisibilityOff,
                                    contentDescription =
                                        if (confirmPasswordVisible) "Скрыть пароль"
                                        else "Показать пароль",
                                    tint = Color.Gray
                                )
                            }
                        }
                    )

                    Spacer(modifier = Modifier.height(24.dp))

                    Button(
                        onClick = {
                            // Я добавил проверку логина на пустое поле
                            if (login.isBlank()) {
                                Toast.makeText(this@SignUp, "Заполните поле Логин", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку длины логина
                            else if (login.length < 8) {
                                Toast.makeText(this@SignUp, "Логин должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку email на пустое поле
                            else if (email.isBlank()) {
                                Toast.makeText(this@SignUp, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку длины email
                            else if (email.length < 12) {
                                Toast.makeText(this@SignUp, "Email должен быть не менее 12 символов", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку чтобы email содержал знак @
                            else if (!email.contains("@")) {
                                Toast.makeText(this@SignUp, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку даты рождения
                            else if (dateOfBirth.isBlank()) {
                                Toast.makeText(this@SignUp, "Выберите дату рождения", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку пароля на пустое поле
                            else if (password.isBlank()) {
                                Toast.makeText(this@SignUp, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку длины пароля
                            else if (password.length < 8) {
                                Toast.makeText(this@SignUp, "Пароль должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку чтобы пароль состоял только из цифр
                            else if (!password.all { it.isDigit() }) {
                                Toast.makeText(this@SignUp, "Пароль должен состоять только из цифр", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку поля повторения пароля
                            else if (confirmPassword.isBlank()) {
                                Toast.makeText(this@SignUp, "Повторите пароль", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил проверку совпадения паролей
                            else if (password != confirmPassword) {
                                Toast.makeText(this@SignUp, "Пароли не совпадают", Toast.LENGTH_SHORT).show()
                            }

                            // Я добавил переход на экран входа и передал туда email и пароль
                            else {
                                val intent = Intent(this@SignUp, LoginIn::class.java)
                                intent.putExtra("email", email)
                                intent.putExtra("password", password)
                                startActivity(intent)
                            }
                        },
                        modifier = Modifier
                            .fillMaxWidth()
                            .height(48.dp)
                            .padding(horizontal = 24.dp),
                        shape = RoundedCornerShape(10.dp),
                        colors = ButtonDefaults.buttonColors(
                            containerColor = Color(0xFF1D61E7)
                        )
                    ) {
                        Text(
                            text = "Зарегистрироваться",
                            fontSize = 15.sp,
                            color = Color.White
                        )
                    }
                }
            }
        }
    }
}


XML

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SignUpLoginInv4">

        <activity
            android:name=".LoginIn"
            android:exported="false" />

        <activity
            android:name=".SignUp"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
```

Architectural Strengths of this Setup
Zero Core Desynchronization: If your backend team updates the password strength requirements, you modify exactly one file inside your shared Kotlin code. The iOS and Android compilation targets will inherit the changes automatically.

Highly Predictable UI: Because the interface state depends strictly on the immutable RegisterUiState snapshot, bugs involving unexpected UI flickering, multiple parallel sign-up requests, or double clicks are structurally eliminated.

Unit Testing Simplified: You can thoroughly write local tests for your registration logic using basic Kotlin unit test tools without mocking UI rendering layouts or hardware environments.


EXAMPLE 5 (XML + LOGIN + SingIN)

```Kotlin
LogIn

package com.example.signuplogininv5

import android.app.DatePickerDialog
import android.content.Context
import android.icu.util.Calendar
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import android.content.Intent
import android.widget.Toast
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.wrapContentSize
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Card
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.geometry.Offset
import androidx.compose.ui.graphics.Brush
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.vector.ImageVector
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.res.vectorResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import java.util.Date

class LoginIn : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            val context = LocalContext.current
            RegistrationScreen(context)
        }
    }

    @OptIn(ExperimentalMaterial3Api::class)
    @Composable
    fun RegistrationScreen(context: Context) {
        var login by remember { mutableStateOf("") }
        var email by remember { mutableStateOf("") }
        var password by remember { mutableStateOf("") }
        var confirmPassword by remember { mutableStateOf("") }

        var passwordVisible by remember { mutableStateOf(false) }
        var confirmPasswordVisible by remember { mutableStateOf(false) }


        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(Color(0xFFEDF1F3))
                .padding(top = 56.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Icon(
                imageVector = ImageVector.vectorResource(R.drawable.vector),
                contentDescription = null,
                tint = Color.Black
            )

            Spacer(modifier = Modifier.height(11.dp))

            //Заголовок
            Text(
                text = "Вход",
                fontSize = 41.sp,
                fontWeight = FontWeight.Bold,
                color = Color(0xFF111827)
            )

            Spacer(modifier = Modifier.height(12.dp))

            Text(
                text = "Войдите, что бы продолжить!",
                fontSize = 12.sp,
                color = Color(0xFF6C7278)
            )

            Spacer(modifier = Modifier.height(24.dp))


            //Поле Email
            Text(
                text = "Email",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp, top = 16.dp)
            )

            OutlinedTextField(
                value = email,
                onValueChange = { email = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp),
                placeholder = { Text("primer@gmail.com", color = Color.Black) },
                keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                shape = RoundedCornerShape(10.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White
                )
            )




            //Поле Пароль
            Text(
                text = "Пароль",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp, top = 16.dp)
            )

            OutlinedTextField(
                value = password,
                onValueChange = { password = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp),
                placeholder = { Text("**********", color = Color.Black) },
                visualTransformation =
                    if (passwordVisible) VisualTransformation.None
                    else PasswordVisualTransformation(),
                keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                shape = RoundedCornerShape(10.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White
                ),
                trailingIcon = {
                    IconButton(onClick = { passwordVisible = !passwordVisible }) {
                        Icon(
                            imageVector = if (passwordVisible) Icons.Default.Visibility else Icons.Default.VisibilityOff,
                            contentDescription = if (passwordVisible) "Скрыть пароль" else "Показать пароль",
                            tint = Color.Gray
                        )
                    }
                }
            )



            Spacer(modifier = Modifier.height(24.dp))


            //Кнопка вход
            Button(
                onClick = {
                    if (login.isBlank()) {
                        Toast.makeText(this@LoginIn, "Заполните поле Логин", Toast.LENGTH_SHORT).show()
                    }
                    // Я добавил проверку длины логина
                    else if (login.length < 8) {
                        Toast.makeText(this@LoginIn, "Логин должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку email на пустое поле
                    else if (email.isBlank()) {
                        Toast.makeText(this@LoginIn, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку длины email
                    else if (email.length < 12) {
                        Toast.makeText(this@LoginIn, "Email должен быть не менее 12 символов", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку чтобы email содержал знак @
                    else if (!email.contains("@")) {
                        Toast.makeText(this@LoginIn, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку пароля на пустое поле
                    else if (password.isBlank()) {
                        Toast.makeText(this@LoginIn, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку длины пароля
                    else if (password.length < 8) {
                        Toast.makeText(this@LoginIn, "Пароль должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку чтобы пароль состоял только из цифр
                    else if (!password.all { it.isDigit() }) {
                        Toast.makeText(this@LoginIn, "Пароль должен состоять только из цифр", Toast.LENGTH_SHORT).show()
                    }
                    else {
                        val intent = Intent(this@LoginIn, SignUp::class.java)
                        intent.putExtra("email", email)
                        intent.putExtra("password", password)
                        startActivity(intent)
                    }



                },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(48.dp)
                    .padding(horizontal = 24.dp),
                shape = RoundedCornerShape(10.dp),
                colors = ButtonDefaults.buttonColors(
                    containerColor = Color(0xFF1D61E7)
                )
            ) {
                Text(
                    text = "Войти",
                    fontSize = 15.sp,
                )
            }

            Row {
                Text(
                    text = "Еще нет аккаунта ?",
                    fontSize = 12.sp,
                    color = Color(0xFF6C7278),
                    textAlign = TextAlign.Center,
                    modifier = Modifier.padding(start = 6.dp, top = 24.dp)
                )

                Text(
                    text = "Регистрация",
                    fontSize = 12.sp,
                    color = Color(0xFF4D81E7),
                    fontWeight = FontWeight.Bold,
                    textAlign = TextAlign.Center,
                    modifier = Modifier
                        .padding(start = 6.dp, top = 24.dp)
                        .clickable {
                            val intent = Intent(this@LoginIn, SignUp::class.java)
                            startActivity(intent)
                        }
                )
            }
        }
    }
}



SignUP

package com.example.signuplogininv5

import android.app.DatePickerDialog
import android.content.Context
import android.icu.util.Calendar
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import android.content.Intent
import android.widget.Toast
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.wrapContentSize
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Card
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.OutlinedTextFieldDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.geometry.Offset
import androidx.compose.ui.graphics.Brush
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.vector.ImageVector
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.res.vectorResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import java.util.Date

class SignUp : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            val context = LocalContext.current
            RegistrationScreen(context)
        }
    }

    @OptIn(ExperimentalMaterial3Api::class)
    @Composable
    fun RegistrationScreen(context: Context) {
        var login by remember { mutableStateOf("") }
        var email by remember { mutableStateOf("") }
        var password by remember { mutableStateOf("") }
        var confirmPassword by remember { mutableStateOf("") }

        var passwordVisible by remember { mutableStateOf(false) }
        var confirmPasswordVisible by remember { mutableStateOf(false) }

        //для работы с датой рождения
        val calendary = Calendar.getInstance()
        val year = calendary.get(Calendar.YEAR)
        val month = calendary.get(Calendar.MONTH)
        val day = calendary.get(Calendar.DAY_OF_MONTH)
        calendary.time = Date()
        var data = remember { mutableStateOf("") }
        val datePicker = DatePickerDialog(
            context, { _, mYear, mMounth, mDayOfMounth ->
                data.value = "$mDayOfMounth.${mMounth + 1}.$mYear"
            }, year, month, day
        )

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(Color(0xFFEDF1F3))
                .padding(top = 56.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Icon(
                imageVector = ImageVector.vectorResource(R.drawable.vector),
                contentDescription = null,
                tint = Color.Black
            )

            Spacer(modifier = Modifier.height(11.dp))

            //Заголовок
            Text(
                text = "Регистрация",
                fontSize = 41.sp,
                fontWeight = FontWeight.Bold,
                color = Color(0xFF111827)
            )

            Spacer(modifier = Modifier.height(12.dp))

            Text(
                text = "Создайте аккаунт, чтобы продолжить!",
                fontSize = 12.sp,
                color = Color(0xFF6C7278)
                )

            Spacer(modifier = Modifier.height(24.dp))

            //Поле Логин
            Text(
                text = "Логин",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp)
            )

            OutlinedTextField(
                value = login,
                onValueChange = { login = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp),
                placeholder = { Text("loginPerson", color = Color.Black) },
                shape = RoundedCornerShape(10.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White
                )
            )

            //Поле Email
            Text(
                text = "Email",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp, top = 16.dp)
            )

            OutlinedTextField(
                value = email,
                onValueChange = { email = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp),
                placeholder = { Text("primer@gmail.com", color = Color.Black) },
                keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
                shape = RoundedCornerShape(10.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White
                )
            )

            //Поле Дата рождения
            Text(
                text = "Дата рождения",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp, top = 16.dp)
            )

            OutlinedTextField(
                value = data.value,
                onValueChange = { },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp)
                    .clickable { datePicker.show() },
                placeholder = { Text("18.03.2003", color = Color.Black) },
                enabled = false,
                shape = RoundedCornerShape(8.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    disabledContainerColor = Color.White,
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White,
                    disabledTextColor = Color.Black, // Цвет текста даже в заблокированном состоянии
                    disabledBorderColor = Color.White // Цвет границы в заблокированном состоянии
                )
            )

            //Поле Пароль
            Text(
                text = "Пароль",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp, top = 16.dp)
            )

            OutlinedTextField(
                value = password,
                onValueChange = { password = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp),
                placeholder = { Text("**********", color = Color.Black) },
                visualTransformation =
                    if (passwordVisible) VisualTransformation.None
                    else PasswordVisualTransformation(),
                keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                shape = RoundedCornerShape(10.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White
                ),
                trailingIcon = {
                    IconButton(onClick = { passwordVisible = !passwordVisible }) {
                        Icon(
                            imageVector = if (passwordVisible) Icons.Default.Visibility else Icons.Default.VisibilityOff,
                            contentDescription = if (passwordVisible) "Скрыть пароль" else "Показать пароль",
                            tint = Color.Gray
                        )
                    }
                }
            )

            //Поле Повторите пароль
            Text(
                text = "Повторите пароль",
                fontSize = 12.sp,
                color = Color(0xFF6C7278),
                modifier = Modifier
                    .align(Alignment.Start)
                    .padding(start = 24.dp, top = 16.dp)
            )

            OutlinedTextField(
                value = confirmPassword,
                onValueChange = { confirmPassword = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 24.dp, end = 24.dp, top = 16.dp),
                placeholder = { Text("**********", color = Color.Black) },
                visualTransformation =
                    if (confirmPasswordVisible) VisualTransformation.None
                    else PasswordVisualTransformation(),
                keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password),
                shape = RoundedCornerShape(10.dp),
                colors = OutlinedTextFieldDefaults.colors(
                    focusedBorderColor = Color.White,
                    unfocusedBorderColor = Color.White,
                    focusedContainerColor = Color.White,
                    unfocusedContainerColor = Color.White
                ),
                trailingIcon = {
                    IconButton(onClick = {
                        confirmPasswordVisible = !confirmPasswordVisible
                    }) {
                        Icon(
                            imageVector = if (confirmPasswordVisible) Icons.Default.Visibility else Icons.Default.VisibilityOff,
                            contentDescription = if (confirmPasswordVisible) "Скрыть пароль" else "Показать пароль",
                            tint = Color.Gray
                        )
                    }
                }
            )

            Spacer(modifier = Modifier.height(24.dp))


            //Кнопка Зарегистрироваться
            Button(
                onClick = {
                    if (login.isBlank()) {
                        Toast.makeText(this@SignUp, "Заполните поле Логин", Toast.LENGTH_SHORT).show()
                    }
                    // Я добавил проверку длины логина
                    else if (login.length < 8) {
                        Toast.makeText(this@SignUp, "Логин должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку email на пустое поле
                    else if (email.isBlank()) {
                        Toast.makeText(this@SignUp, "Заполните поле Email", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку длины email
                    else if (email.length < 12) {
                        Toast.makeText(this@SignUp, "Email должен быть не менее 12 символов", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку чтобы email содержал знак @
                    else if (!email.contains("@")) {
                        Toast.makeText(this@SignUp, "Email должен содержать символ @", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку пароля на пустое поле
                    else if (password.isBlank()) {
                        Toast.makeText(this@SignUp, "Заполните поле Пароль", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку длины пароля
                    else if (password.length < 8) {
                        Toast.makeText(this@SignUp, "Пароль должен быть не менее 8 символов", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку чтобы пароль состоял только из цифр
                    else if (!password.all { it.isDigit() }) {
                        Toast.makeText(this@SignUp, "Пароль должен состоять только из цифр", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку поля повторения пароля
                    else if (confirmPassword.isBlank()) {
                        Toast.makeText(this@SignUp, "Повторите пароль", Toast.LENGTH_SHORT).show()
                    }

                    // Я добавил проверку совпадения паролей
                    else if (password != confirmPassword) {
                        Toast.makeText(this@SignUp, "Пароли не совпадают", Toast.LENGTH_SHORT).show()
                    }
                    // Я добавил переход на экран входа и передал туда email и пароль
                    else {
                        val intent = Intent(this@SignUp, LoginIn::class.java)
                        intent.putExtra("email", email)
                        intent.putExtra("password", password)
                        startActivity(intent)
                    }


                },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(48.dp)
                    .padding(horizontal = 24.dp),
                shape = RoundedCornerShape(10.dp),
                colors = ButtonDefaults.buttonColors(
                    containerColor = Color(0xFF1D61E7)
                )
            ) {
                Text(
                    text = "Зарегистрироваться",
                    fontSize = 15.sp,
                )
            }

            Row {
                Text(
                    text = "Уже есть аккаунт?",
                    fontSize = 12.sp,
                    color = Color(0xFF6C7278),
                    textAlign = TextAlign.Center,
                    modifier = Modifier.padding(start = 6.dp, top = 24.dp)
                )

                Text(
                    text = "Вход",
                    fontSize = 12.sp,
                    color = Color(0xFF4D81E7),
                    fontWeight = FontWeight.Bold,
                    textAlign = TextAlign.Center,
                    modifier = Modifier
                        .padding(start = 6.dp, top = 24.dp)
                        .clickable {
                            val intent = Intent(this@SignUp, LoginIn::class.java)
                            startActivity(intent)
                        }
                )
            }
        }
    }
}


XML

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.SignUpLoginInv5">
        <activity
            android:name=".LoginIn"
            android:exported="false" />
        <activity
            android:name=".SignUp"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.SignUpLoginInv5">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

```

