# 🌐 Local Language Setup

> **Configure Ubuntu to type in your native language with proper keyboard layouts and input methods**

## 📋 Table of Contents

- [🚀 Quick Setup](#-quick-setup)
- [📥 Installation](#-installation)
- [🔧 System Configuration](#-system-configuration)
- [⌨️ Keyboard Layout Setup](#️-keyboard-layout-setup)
- [✍️ Input Methods](#️-input-methods)
- [🎯 Language-Specific Tips](#-language-specific-tips)
- [🔤 Special Characters](#-special-characters)

---

## 🚀 Quick Setup

This guide helps you set up local language typing (Hindi, Kannada, Tamil, Bengali, etc.) on Ubuntu Linux using the m17n input method.

### What You'll Learn
- Install and configure multilingual input support
- Set up your preferred local language keyboard
- Switch between English and local language typing
- Use special keystroke combinations for accurate typing

---

## 📥 Installation

### Step 1: Update System

```bash path=null start=null
# Update package lists and upgrade system
sudo apt update
sudo apt upgrade -y
```

### Step 2: Install m17n Input Method

```bash path=null start=null
# Install the m17n multilingual input method
sudo apt-get install ibus-m17n
```

> 💡 **What is m17n?** The m17n library provides multilingual text processing capabilities, supporting various scripts and input methods for different languages.

---

## 🔧 System Configuration

### Configure System Locales

```bash path=null start=null
# Configure system locales
sudo dpkg-reconfigure locales
```

#### 🔧 Locale Selection Process

1. **Navigate the Locale List**
   - Use **arrow keys** to move up and down
   - Use **spacebar** to select/deselect locales

2. **Select Your Language Locale**

   Common examples:
   ```
   kn_IN UTF-8    # Kannada (India)
   hi_IN UTF-8    # Hindi (India) 
   ta_IN UTF-8    # Tamil (India)
   te_IN UTF-8    # Telugu (India)
   bn_IN UTF-8    # Bengali (India)
   gu_IN UTF-8    # Gujarati (India)
   pa_IN UTF-8    # Punjabi (India)
   mr_IN UTF-8    # Marathi (India)
   ```

3. **Navigation Controls**
   - Press **Tab** to move to "OK" button
   - Press **Enter** to confirm selection

4. **Set Default System Locale**
   - Keep **en_US.UTF-8** as the default system locale
   - This ensures system messages remain in English while supporting your local language for input

5. **Complete Configuration**
   - Press **Tab** to navigate to "OK"
   - Press **Enter** to finish

### Reboot System

```bash path=null start=null
# Reboot to apply locale changes
sudo reboot
```

---

## ⌨️ Keyboard Layout Setup

### Access Language Settings

1. **Open Settings**
   - Click on Settings in your applications menu
   - Or press `Super` key and type "Settings"

2. **Navigate to Region & Language**
   - Go to "Region & Language" section

3. **Add Input Sources**
   - Click on "+" to add a new input source
   - Search for your language

### Find Your Language Layout

Look for your language with the **itrans (m17n)** suffix:

```
Examples:
- Kannada (kn-itrans (m17n))
- Hindi (hi-itrans (m17n))
- Tamil (ta-itrans (m17n))
- Bengali (bn-itrans (m17n))
```

> 💡 **Why itrans?** The itrans (Indian Language Transliteration) method is often the most convenient for typing Indian languages using English keyboard layouts.

### Alternative Input Methods

You can also try these methods for your language:
- **Inscript**: Government standard keyboard layout
- **Phonetic**: Based on pronunciation
- **Traditional**: Language-specific traditional layouts

---

## ✍️ Input Methods

### Switching Between Languages

- **Super + Space**: Switch between input methods
- **Language Indicator**: Click the language indicator in the top bar
- **Settings Shortcut**: Set custom shortcuts in Keyboard settings

### Essential Keystrokes

#### Special Character Input

For many Indian languages, certain characters require special key combinations:

```bash path=null start=null
# Example for Kannada (and similar for other languages)
Shift + M = Insert 'o' vowel mark (ಂ)

# This creates characters like:
# ಅ + Shift+M = ಅಂ (am sound)
```

#### Common Patterns

Most itrans input methods follow these patterns:

```
Vowels:
a = ಅ    aa/A = ಆ
i = ಇ    ii/I = ಈ  
u = ಉ    uu/U = ಊ
e = ಎ    ee/E = ಏ
o = ಒ    oo/O = ಓ

Consonants:
ka = ಕ   kha = ಖ   ga = ಗ   gha = ಘ
cha = ಚ  chha = ಛ  ja = ಜ   jha = ಝ
```

---

## 🎯 Language-Specific Tips

### Hindi (Devanagari)
```
Common combinations:
- ka = क    kha = ख    ga = ग
- cha = च   chha = छ   ja = ज
- ra = र    la = ल     va = व
```

### Tamil
```
Common combinations:
- ka = க    cha = ச    ta = த
- pa = ப    ma = ம     ya = ய
- ra = ர    la = ல     va = வ
```

### Bengali
```
Common combinations:
- ka = ক    kha = খ    ga = গ
- cha = চ   chha = ছ   ja = জ
- ra = র    la = ল     ba = ব
```

### Kannada
```
Common combinations:
- ka = ಕ    kha = ಖ    ga = ಗ
- cha = ಚ   chha = ಛ   ja = ಜ
- ra = ರ    la = ಲ     va = ವ
```

---

## 🔤 Special Characters

### Conjunct Characters

Many Indian languages use conjunct characters (combined consonants):

```bash path=null start=null
# General pattern for conjuncts
consonant1 + 'a' (halant) + consonant2

# Examples (varies by language):
# k + virama + ta = kta conjunct
# s + virama + va = sva conjunct
```

### Numbers and Symbols

```bash path=null start=null
# English numbers in local script
0-9 types as ०-९ (Devanagari) or equivalent in your script

# Common punctuation
. (period) = । (danda)
? = ?  (usually remains the same)
! = !  (usually remains the same)
```

---

## 🔧 Testing Your Setup

### Verify Language Input

1. **Open a text editor** (gedit, LibreOffice Writer, etc.)

2. **Switch to your language input**
   - Press `Super + Space` or click language indicator

3. **Test typing**
   - Type simple words in your language
   - Test special character combinations
   - Try numbers and punctuation

### Sample Test Phrases

**Hindi**: namaste kaise hain aap
**Kannada**: namaskara hegiddira
**Tamil**: vanakkam eppadi irukkireerkal
**Bengali**: namoshkar kemon achen

---

## 🛠️ Troubleshooting

### Common Issues

#### Language Not Appearing
```bash path=null start=null
# Reinstall ibus-m17n
sudo apt remove ibus-m17n
sudo apt install ibus-m17n

# Restart ibus
ibus restart
```

#### Input Method Not Working
```bash path=null start=null
# Check if ibus is running
ps aux | grep ibus

# Start ibus if not running
ibus-daemon -drx
```

#### Wrong Characters Appearing
- **Check Layout**: Ensure you selected the correct layout (itrans vs inscript)
- **Restart Application**: Sometimes restarting the text application helps
- **System Reboot**: A full reboot often resolves input method issues

### Reset Input Method

```bash path=null start=null
# Reset ibus configuration
rm -rf ~/.config/ibus
ibus-daemon -drx

# Reconfigure language settings in System Settings
```

---

## 💡 Pro Tips

### Productivity Enhancements

1. **Learn Keyboard Shortcuts**
   - Memorize `Super + Space` for quick language switching
   - Set up custom shortcuts for frequently used layouts

2. **Use Spell Check**
   - Enable spell checking in applications like LibreOffice
   - Install language-specific dictionaries when available

3. **Font Optimization**
   - Install language-specific fonts for better rendering
   - Popular fonts: Noto Sans [Language], Lohit, Mangal

4. **Practice Common Combinations**
   - Create a practice document with common words
   - Practice special keystrokes like `Shift+M` regularly

### Advanced Configuration

```bash path=null start=null
# Install language-specific fonts
sudo apt install fonts-noto-core fonts-noto-ui-core

# For specific languages, install dedicated font packages:
# sudo apt install fonts-lohit-deva     # Hindi/Marathi
# sudo apt install fonts-lohit-knda     # Kannada  
# sudo apt install fonts-lohit-taml     # Tamil
# sudo apt install fonts-lohit-beng     # Bengali
```

---

## 🌟 Best Practices

1. **Start Simple**: Begin with basic words and gradually learn complex combinations

2. **Consistent Practice**: Regular typing practice improves speed and accuracy

3. **Learn Phonetic Patterns**: Understanding the transliteration logic helps with unfamiliar words

4. **Use Reference Cards**: Keep a reference of common character combinations handy

5. **Backup Settings**: Export your input method settings for easy restoration

---

> 🎯 **Success Tip**: The itrans method is designed to be intuitive - type what you hear! Most words can be typed phonetically and will appear correctly in your local script.

**Next Steps**: Once you're comfortable with basic typing, explore [Advanced Productivity Features](workflow-optimization.md) or set up [Development Environment](../development/) with multilingual support.
