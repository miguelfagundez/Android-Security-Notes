# Security Notes
Some notes about basic security in android development

## EncryptedFile & EncryptedSharedPreferences

**Reference**: `androidx.security.crypto`

Complete docs [here](https://developer.android.com/reference/androidx/security/crypto/package-summary)

### Read / Write files using `EncryptedFile`

These classes are used to create and read encrypted files. Using *EncryptedFile* and *EncryptedSharedPreferences* allows you to locally protect files that may contain sensitive data, API keys, OAuth tokens, and other types of secrets.

First, we need to generate a symetric key using `MasterKey.Builder`, and this key is stored in the `Android Keystore system`.

```kotlin
 val masterKey = MasterKey.Builder(context)
                  .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
                  .build()
```

Second, we create the file and use the MasterKey.

```kotlin
    val file = File(context.filesDir, "secret_data")

    val encryptedFile = EncryptedFile.Builder(
        context,
        file,
        masterKey,
        EncryptedFile.FileEncryptionScheme.AES256_GCM_HKDF_4KB
    ).build()
```

Finally, we can write or read our file.

```kotlin
// Checking if file exist before write
if (fileToWrite.exists()) {
  fileToWrite.delete()
}
// write to the encrypted file
 val encryptedOutputStream = encryptedFile.openFileOutput().apply{
  ...
 }

 // read the encrypted file
 val encryptedInputStream = encryptedFile.openFileInput().read()
```
