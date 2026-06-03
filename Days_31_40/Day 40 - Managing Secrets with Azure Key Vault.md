The Nautilus DevOps team is focusing on improving their data security by using Azure Key Vault. Your task is to create a Key Vault with an RSA key and manage the encryption and decryption of a pre-existing sensitive file using this key.

Specific Requirements:

1. Create key vault:
	- Name the Key Vault `xfusion-26357`.
	- Create the Vault in the East US region.
	- Use the Standard pricing tier.
	- Set Soft Delete Retention to 7 Days.
	- Use the Vault access policy permission model.
	- Configure an access policy that allows Get, List, Encrypt, and Decrypt permissions for the lab idenity.

2. Create an RSA Key:
	- Create a key named `xfusion-key` within the Key Vault.
	- Key type: RSA.
	- RSA key size: 4096.
	- Leave all other settings as default.

3. Encrypt the Sensitive Data:
	- Use the key to encrypt the provided `SensitiveData.txt` file (located in `/root/`) on the `azure-client`.
	- Use the `RSA-OAEP` algorithm.
	- Base64 encode the plaintext before encryption.
	- Save the encrypted version as `EncryptedData.bin` in the `/root/` directory.

**Note**: If you encounter a permissions error, retrieve the Service Principle ID using: `az account show --query user.name -o tsv` and grant the required Key Vault permissions.

4. Verify Decryption:
	- Decrypt `EncryptedData.bin`.
	- Base64 decode the decrypted output.
	- Save the result as `DecryptedData.txt` in `/root/`.
	- Ensure the decrypted file matches the original `SensitiveData.txt`.

Ensure that the Key Vault and key are correctly configured. The validation script will test your configuration by decrypting the `EncryptedData.bin` file using the key you created.

**Notes**:
- Create the resource only in the East US region.
- Network restrictions or private endpoints are NOT required for this task. 

### Answer using Azure Portal

1. Create Key Vault:
	1. Login with the credentials provided.
	2. In the top search bar, search for key vault and click on Key Vaults.
	3. Click + Create to create the key vault with the specific configuration.
		- Name = `xfusion-26357`
		- Region = East US
		- Pricing tier = Standard
		- Day to retain deleted vaults 7
		- Permission model = Vault access policy
		- Access policies = select Lab identity and edit permissions to Get, List, Create, Encrypt, and Decrypt 
	4. Once complete, press review and create.
	5. Once created, go to resource.

2.  Create RSA Key:
	1. On the overview page under Objects, click on Keys.
	2. Click + Generate/Import.
	3. Use the following details;
		- Named = `xfusion-key`.
		- Key type: RSA.
		- RSA key size: 4096.
		- Leave all other settings as default.

3. Encrypt the Sensitive Data:
	1. Use the following command to encrypt the local file;

```
# Encrypt contents in base64
cat SensitiveDAta.txt | base64

# Encrypt with Key from Key vault
az keyvault key encrypt \
--name xfusion-key \
--vault-name xfusion-26357 \
--alogrithm RSA-OAEP \
--value <base64 output> \
--data-type base64

# Copy the Results from the encryption and save to file
echo <results-value> > EncryptedData.bin
```

4. Decrypt the Sensitive Data:
	1. To decrypt the value use the following commands;

```
az keyvault key decrypt \
--name xfusion-key \
--vault-name xfusion-26357 \
--alogrithm RSA-OAEP \
--data-type base64 \
--value <results-value from encryption>

# Decrypted value now needs to be decrypted from base64
base64 -d <<< value

# Output should be the same as in SensitiveData.txt

# To save into file we can use the following;
base64 -d <<< value > DecryptedData.txt
```

5. Click check to complete the task.
