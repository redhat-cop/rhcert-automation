# Ansible Playbook for CCSP Certification

This README file provides instructions on how to run the Ansible playbook for creating a partner product in RHcert SPA.

## Prerequisites

1. Ansible must be installed on your system. If not installed, please follow the instructions provided in the Ansible documentation: [Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html).

2. You need to have access to [RHcert SPA (Red Hat Certification)](|https://rhcert.connect.redhat.com/) to create a partner product.

3. You will need an authentication token to access the Red Hat Ansible Automation Hub API. Follow the steps below to obtain the token:

   - Visit [Red Hat Ansible Automation Hub](https://console.redhat.com/ansible/automation-hub/token).
   - Click on "Load token" for offline token generation.
   - Copy the generated token.

## Parameter

1. CertificationTypeId

| Certification Name                     | ID   |
| -------------------------------------- | ---- |
| Red Hat Enterprise Linux 9 Cloud Image | 1297 |
| Red Hat Enterprise Linux 8 Cloud Image | 207  |

2. PartnerProductID
   Visit [RHcert SPA (Red Hat Certification)](https://rhcert.connect.redhat.com/#/products) to check existing partner and use ID of a Cloud Image Partner

3. UserToken
   Visit [Red Hat Ansible Automation Hub](https://console.redhat.com/ansible/automation-hub/token) and follow instruction mentioned above

## Running the Ansible Playbook

Clone this playbook to your local machine and save as playbook file playbook.yml.

Open a terminal and navigate to the directory containing the playbook file.

Run the following command to execute the playbook:

`ansible-playbook playbook.yml -e "CertificationTypeId=<certification_type_id> PartnerProductId=<partner_product_id> UserToken=<offline_token>"`

## CCSP Ansible Playbook Steps

1. **Setup Playbook**: Start the playbook named "CCSP Ansible Playbook" and target the localhost.

2. **Install Required Packages**: Ensure the latest versions of several RPM packages (like insights-client, podman, uuidd, etc.) are installed on the system.

3. **Restart Services**: Restart the services `nftables` and `firewalld`. Ignore any errors that occur during this process.

4. **Check for Package**: Run a shell command to check if the `redhat-cert` package is installed and save the output for later use. Ignore errors if the package is not found.

5. **Debug Package Check**: Output the result of the package check to the debug console.

6. **Clean rhcert-cli**: Execute a command to clean all configurations using `rhcert-cli`.

7. **Restart rhcertd**: Restart the `rhcertd` service and ignore any errors.

8. **Get Access Token**: Make a POST request to Red Hat's SSO to get an access token using a refresh token provided by the user.

9. **Check Token Refresh**: Fail the playbook with a message if the token refresh was not successful.

10. **Create Certification Case**: Send a POST request to create a new certification case in Red Hat's system using the obtained access token and provided product and certification details.

11. **Debug Case Number**: Output the case number to the debug console if it was successfully created.

12. **Provision & Run Tests**: Run the `rhcert-cli` command to provision and run tests automatically, and capture the output.

13. **Debug Provisioning**: Output the results of the provisioning and run command to the debug console if any output is available.

14. **Save Test Result**: Run the `rhcert-cli save` command to save the test results and capture the output.

15. **Debug Save Command**: Output the results of the save command to the debug console if any output is available.

16. **Extract Filename**: Extract the file path of the saved test results from the previous command output using a shell command and capture it.

17. **Debug Filename**: Output the extracted file path to the debug console if available.

18. **Pause**: Wait for one minute to ensure the test results are fully generated.

19. **Debug Case Upload**: Output a message indicating that the test result file is being uploaded for the created case.

20. **Get File Stats**: Check the file stats of the saved test result file to ensure it exists and capture the stats.

21. **Debug File Existence**: Output a message to the debug console if the file path does not exist.

22. **Get Access Token Again**: Make another POST request to get a fresh access token from Red Hat's SSO using the refresh token.

23. **Check Token Refresh Again**: Fail the playbook with a message if the second token refresh was not successful.

24. **Upload Test Result**: Use the `curl` command to upload the test result file to Red Hat's certification system with the obtained access token.

25. **Debug File Upload**: Output the result of the file upload to the debug console if available.

26. **Open Certification Case Link**: Output a link to the created certification case in Red Hat's system if the case number is available.

## Support

If you encounter any issues or have any questions regarding the playbook or its execution, please feel free to reach out sridhal@redhat.com, sudeshmu@redhat.com
for assistance.
