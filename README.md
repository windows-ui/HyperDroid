# HyperDroid - PC Launcher
Native desktop experience on your android device.

## Get
<a href="https://play.google.com/store/apps/details?id=com.binary.hyperdroid">
  <img src="src/assets/playstore_link.svg" alt="Build" />
</a>

<table>
  <thead>
    <tr>
      <th colspan="2">Screenshots</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;">
        <img src="src/assets/screen_1.webp" alt="App's main screen with StartMenu" width="350">
      </td>
      <td style="text-align: center;">
        <img src="src/assets/screen_4.webp" alt="Settings system" width="350">
      </td>
    </tr>
    <tr>
      <td style="text-align: center;">
        <img src="src/assets/screen_5.webp" alt="File Manager" width="350">
      </td>
      <td style="text-align: center;">
        <img src="src/assets/screen_3.webp" alt="Settings colors" width="350">
      </td>
    </tr>
  </tbody>
</table>
import android.app.admin.DevicePolicyManager
import android.content.ComponentName
import android.content.Context
import android.os.PowerManager
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

// === CONNECTION BRIDGE (Actual physical screen off system) ===
fun triggerSystemSleep(context: Context) {
    val powerManager = context.getSystemService(Context.POWER_SERVICE) as PowerManager
    val devicePolicyManager = context.getSystemService(Context.DEVICE_POLICY_SERVICE) as DevicePolicyManager
    // Note: In real app, replace AdminReceiver class with your registered Receiver
    val adminComponent = ComponentName(context, "MyDeviceAdminReceiver")

    if (powerManager.isInteractive) {
        if (devicePolicyManager.isAdminActive(adminComponent)) {
            devicePolicyManager.lockNow() // Forces physical screen to turn off
        }
    }
}

// === MAIN UI CONNECTED TOGETHER (Lock screen integrated with sleep system) ===
@Composable
fun WindowsConnectedLockScreen() {
    val context = LocalContext.current
    var passwordInput by remember { mutableStateOf("") }
    var isScreenDark by remember { mutableStateOf(false) }
    val correctPassword = "1234"

    val backgroundColor = if (isScreenDark) Color.Black else Color(0xFF1C1C1C)

    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(backgroundColor),
        contentAlignment = Alignment.Center
    ) {
        if (!isScreenDark) {
            Column(
                horizontalAlignment = Alignment.CenterHorizontally,
                verticalArrangement = Arrangement.Center,
                modifier = Modifier.padding(24.dp)
            ) {
                Text(
                    text = "Windows Security",
                    color = Color.White,
                    fontSize = 28.sp,
                    modifier = Modifier.padding(bottom = 32.dp)
                )

                OutlinedTextField(
                    value = passwordInput,
                    onValueChange = { passwordInput = it },
                    label = { Text("Enter Password", color = Color.LightGray) },
                    visualTransformation = PasswordVisualTransformation(),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White,
                        focusedBorderColor = Color.Cyan,
                        unfocusedBorderColor = Color.Gray
                    ),
                    modifier = Modifier.width(280.dp)
                )

                Spacer(modifier = Modifier.height(16.dp))

                Row {
                    // Button 1: UNLOCK SYSTEM
                    Button(
                        onClick = {
                            if (passwordInput == correctPassword) {
                                passwordInput = "" // Success action
                            }
                        },
                        colors = ButtonDefaults.buttonColors(containerColor = Color(0xFF0078D4)),
                        modifier = Modifier.padding(end = 8.dp)
                    ) {
                        Text("Unlock", color = Color.White)
                    }

                    // Button 2: GO DARK AND TRIGGER SLEEP (Connected bridge for physical screen sleep)
                    Button(
                        onClick = { 
                            isScreenDark = true
                            triggerSystemSleep(context) // Triggers the physical screen off
                        },
                        colors = ButtonDefaults.buttonColors(containerColor = Color.DarkGray)
                    ) {
                        Text("Sleep", color = Color.White)
                    }
                }
            }
        } else {
            // Wake up detection area
            Button(
                onClick = { isScreenDark = false },
                colors = ButtonDefaults.buttonColors(containerColor = Color.Transparent),
                modifier = Modifier.fillMaxSize()
            ) {}
        }
    }
}

## Features

### ✨ **User-Friendly Interface**  
<sup>Intuitive desktop-style interface optimized for both touch input and external peripherals (keyboard, mouse, etc.).</sup>

### 🪟 **Floating Window Support**  
<sup>Launch installed WebApps in resizable, floating windows for enhanced multitasking.</sup>

### ⚙️ **Customizable Display Density**  
<sup>Adjust screen scaling to suit your preferences and display size.</sup>

### 🌍 **Multi-Language Support**  
<sup>Available in multiple languages to ensure a more accessible experience for all users.</sup>

### 📁 **Built-in File Manager**  
<sup>Manage your files easily with a clean and user-friendly file explorer.</sup>

### 🎨 **Editable App Icons and Names**  
<sup>Personalize your experience by renaming apps and changing their icons.</sup>

### 📱 **Set as Default Launcher**  
<sup>Optionally set **HyperDroid** as your system’s default launcher for seamless integration.</sup>

## Links
<a href="https://github.com/windows-ui/HyperDroid/tree/main/PrivacyPolicy">
  <img src="https://img.shields.io/badge/Privacy%20Policy-View-blue?style=for-the-badge&logo=polywork&logoColor=white" style="height:40px;">
</a>
