# Screenshots Evidence Folder

```text
Screenshot: 01-entra-custom-domain-verified.png
Description: Shows the custom domain `ogkareemu.live` successfully added and verified in Microsoft Entra ID, enabling the lab tenant to use a professional UPN/domain suffix instead of the default `.onmicrosoft.com` domain.
```
<img width="675" height="394" alt="Screenshot 2026-07-05 at 08 38 59" src="https://github.com/user-attachments/assets/ed8d7074-65ec-43aa-9ce4-255a169edde7" />


```text
Screenshot: 02-ad-upn-suffix.png
Description: Shows the custom UPN suffix `ogkareemu.live` added in Active Directory Domains and Trusts, allowing on-premises AD users to be assigned a cloud-friendly sign-in name that matches the verified Entra ID domain.
```
<img width="482" height="558" alt="Screenshot 2026-07-05 at 08 58 32" src="https://github.com/user-attachments/assets/f3fa639f-34dc-40b5-99f7-76057a1bb4ab" />


```text
Screenshot: 03-ad-user-upn.png
Description: Shows a test user account in on-premises Active Directory configured with the `ogkareemu.live` UPN suffix, preparing the account for hybrid identity synchronisation to Microsoft Entra ID.
```
<img width="472" height="78" alt="Screenshot 2026-07-05 at 08 54 29" src="https://github.com/user-attachments/assets/382f910c-398a-4109-a9c9-56da9a91477b" />


```text
Screenshot: 04-entra-connect-ou-filtering.png
Description: Shows Microsoft Entra Connect configured with OU filtering, ensuring only selected Active Directory organisational units are synchronised to Microsoft Entra ID as part of the hybrid identity lab.
```
**EV-1**
 <img width="777" height="496" alt="Screenshot 2026-07-05 at 09 01 20" src="https://github.com/user-attachments/assets/0605645a-5d23-452f-b216-bc89d077d72e" />

 **EV-2**
 <img width="842" height="613" alt="Screenshot 2026-07-05 at 09 01 51" src="https://github.com/user-attachments/assets/bd4d16a3-98d9-4040-8316-ea584b21b041" />


```text
Screenshot: 05-sync-service-export-success.png
Description: Shows a successful export operation in the Microsoft Entra Connect Synchronisation Service Manager, confirming that changes from on-premises Active Directory were successfully exported to Microsoft Entra ID.
```
<img width="938" height="226" alt="Screenshot 2026-07-05 at 09 05 20" src="https://github.com/user-attachments/assets/9b96da21-b01a-452a-b6a7-741b9654549a" />


```text
Screenshot: 06-entra-synced-user.png
Description: Shows a test user synchronised from on-premises AD to Microsoft Entra ID with On-premises sync enabled set to Yes and UPN using `ogkareemu.live`.
```
<img width="989" height="848" alt="Screenshot 2026-07-05 at 09 08 52" src="https://github.com/user-attachments/assets/d493e180-73e6-44c8-9de2-45bddb9fd2cf" />


```text
Screenshot: 07-provisioning-agent-health.png
Description: Shows the Microsoft Entra provisioning agent in a healthy state, confirming that the on-premises environment is connected and available for lifecycle workflow actions involving Active Directory.
```
<img width="977" height="461" alt="Screenshot 2026-07-05 at 09 16 13" src="https://github.com/user-attachments/assets/1867797f-1111-44f6-945e-e42659375113" />


```text
Screenshot: 08-lifecycle-workflow-config.png
Description: Shows the Microsoft Entra ID Governance lifecycle workflow configuration used to automate identity lifecycle actions, such as processing leaver activity for hybrid user accounts.
```
**EV-1**
<img width="989" height="428" alt="Screenshot 2026-07-05 at 09 18 54" src="https://github.com/user-attachments/assets/0581792f-9cf9-420c-9f36-20f92c9d6e26" />

**EV-2**
<img width="979" height="521" alt="Screenshot 2026-07-05 at 09 19 16" src="https://github.com/user-attachments/assets/123c0a3e-fd14-4430-a54a-e2fdc67e7b49" />


```text
Screenshot: 09-leaver-disabled-user.png
Description: Shows the on-premises Active Directory test user account disabled after the lifecycle workflow ran, demonstrating automated leaver processing in a hybrid identity environment.
```
**EV-1**
<img width="631" height="459" alt="Screenshot 2026-07-05 at 09 20 42" src="https://github.com/user-attachments/assets/657593d8-0fdc-4356-8fc5-0f8296733b9c" />

**EV-2**
<img width="708" height="784" alt="Screenshot 2026-07-05 at 09 21 39" src="https://github.com/user-attachments/assets/6361dcdd-81e7-4423-af96-5e04311429b9" />


```text
Screenshot: 10-sign-in-test.png
Description: Shows a sign-in test for the synchronised user account, validating the end-to-end hybrid identity setup from on-premises AD synchronisation to Microsoft Entra ID authentication.
```
<img width="1058" height="551" alt="Screenshot 2026-07-05 at 09 33 45" src="https://github.com/user-attachments/assets/aee110a1-06cb-4f0c-a22b-ea11b4437d6e" />
