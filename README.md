# Claw-Enterprises---Cloud-Engineer-Assessment

## Task 1


### VNet, Ubuntu VM, and Secure Configuration

1. Set up a Virtual Network (VNet) in Azure.
![App Screenshot](./task-1/Screenshot%20(3).png)

2. Deploy an Ubuntu VM within the VNet.
![App Screenshot](./task-1/Screenshot%20(4).png)
![App Screenshot](./task-1/Screenshot%20(6).png)

3. Ensure the VM is accessible via SSH, but use Network Security Groups (NSGs) to restrict access to only your IP address.
- I done this as showing in the screenshot. but  i am working with wifi network, in which ip changing regularly so i change it "any to any"
![App Screenshot](./task-1/Screenshot%20(7).png)

4. Keep the inbound ports for HTTP and HTTPS open and publicly accessible.
![App Screenshot](./task-1/Screenshot%20(9).png)


### Additional Requirements:

- Enable Azure Security Center for the VM.
- Configure a custom DNS server for the VNet.

## Task 2.

### Advanced Storage Solution

1. Create an Azure storage account.
![App Screenshot](./task-2/Screenshot%20(10).png)
2. Set up an Azure CLI on the VM
```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```
![App Screenshot](./task-2/Screenshot%20(8).png)
3. From your Ubuntu VM, upload the index.html to the container.
- To copy from VM to Azure Storage Account Container , We are using AzCopy tool.
- we need to install and config the AzCopy Tool-[Documentation](https://gist.github.com/aessing/76f1200c9f5b2b9671937b3b0ed5fd6f)
```bash
# Download and extract
wget https://aka.ms/downloadazcopy-v10-linux
tar -xvf downloadazcopy-v10-linux

# Move AzCopy
sudo rm -f /usr/bin/azcopy
sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
sudo chmod 755 /usr/bin/azcopy

# Clean the kitchen
rm -f downloadazcopy-v10-linux
rm -rf ./azcopy_linux_amd64_*/
```
- To make container in the Storage account
```bash
azcopy make "https://clawenterprisesstorage1.blob.core.windows.net/claw-enterprises-container?sv=2022-11-02&ss=bfqt&srt=sc&sp=rwdlacupiytfx&se=2024-07-17T00:43:51Z&st=2024-07-15T15:43:51Z&spr=https&sig=xsbnzef297hWDKCLMAbSfrIKNlxHmEI%2Foms%2BRTVikSc%3D"
```
![App Screenshot](./task-2/Screenshot%20(13).png)
- To copy index.html file from VM to Storage account container

```bash
azcopy copy "/home/satish/html/index.html" "https://clawenterprisesstorage1.blob.core.windows.net/claw-enterprises-container?sp=racwdli&st=2024-07-15T17:25:24Z&se=2024-07-16T06:29:00Z&spr=https&sv=2022-11-02&sr=c&sig=wfACrylkB7Ri55xORjmG0kP0Z5SU%2FpbhYEgW24cs5ak%3D"
```
![App Screenshot](./task-2/Screenshot%20(16).png)


## Task 3.

### Advanced Application Deployment

1. Fork the example repository [here](https://github.com/render-examples/flask-hello-world/tree/master).

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

2. Deploy the code on the VM.
- Install Required Dependencies
Flask is a python-based application. So Python and other required dependencies must be installed on your server. If not installed you can install all of them with the following command:

```bash
apt-get install python3 python3-pip python3-dev build-essential libssl-dev libffi-dev python3-setuptools -y


```
Error message:
```
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.

    If you wish to install a non-Debian-packaged Python package,
    create a virtual environment using python3 -m venv path/to/venv.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
    sure you have python3-full installed.

    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.

    See /usr/share/doc/python3.12/README.venv for more information.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.

```
Solution:-
```bash
apt install pipx

pipx install some-python-application
```
Once all the dependencies are installed, install the Python virtual environment package using the following command:

```bash
apt-get install python3-venv -y
```

3. Create a system service to automatically start the application whenever the VM restarts.
4. Configure NGINX as a reverse proxy for the Flask application.