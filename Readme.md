Sure! Here’s the **English translation** of your `README.md` file:  

---

# **Instructions to Set Up Docker in Codespace**  

## 1. **Check Available Storage**  
In the terminal, run the following command to view partitions and available storage:  

```bash
df -h
```  

Choose the partition with the most available storage.  

## 2. **Create a Folder for Docker**  
Run the following command to create the folder where Docker will store data:  

```bash
sudo mkdir -p /tmp/docker-data
```  

## 3. **Configure Docker**  
Now, edit the Docker configuration file:  

```bash
sudo nano /etc/docker/daemon.json
```  

Paste the following content:  

```json
{
  "data-root": "/tmp/docker-data"
}
```  

## 4. **Restart Your Codespace**  
Turn your Codespace off and on again to apply the changes.  

## 5. **Verify the Configuration**  
To ensure Docker is correctly configured, run:  

```bash
docker info
```  

---

## **Create `windows10.yml` File**  

Create a file named `windows10.yml` with the following content:  

```yaml
services:
  windows:
    image: dockurr/windows
    container_name: windows
    environment:
      VERSION: "10"
      USERNAME: ${WINDOWS_USERNAME}   # Use a .env file for sensitive variables
      PASSWORD: ${WINDOWS_PASSWORD}   # Use a .env file for sensitive variables
      RAM_SIZE: "4G"
      CPU_CORES: "4"
    cap_add:
      - NET_ADMIN
    ports:
      - "8006:8006"
      - "3389:3389/tcp"  # Expose only TCP for RDP
    volumes:
      - /tmp/docker-data:/mnt/disk1   # Ensure this directory exists
      - windows-data:/mnt/windows-data # Additional mount if needed
    devices:
      - "/dev/kvm:/dev/kvm"  # Only if you need KVM access
      - "/dev/net/tun:/dev/net/tun"  # Only if you need virtual network interfaces
    stop_grace_period: 2m
    restart: always

volumes:
  windows-data:  # Define the volume if it doesn't exist
```  

---

## **Create `.env` File**  

Create a `.env` file in the same folder as `windows10.yml` to define sensitive environment variables, such as username and password:  

```ini
WINDOWS_USERNAME=YourUsername
WINDOWS_PASSWORD=YourPassword
```  

This file **should not** be uploaded to public repositories for security reasons. Make sure to add it to your `.gitignore` file if you're using version control.  

---

## **Start the Container**  

1. **Run the container** by executing the following command:  

   ```bash
   docker-compose -f windows10.yml up
   ```

2. **Start the container** with:  

   ```bash
   docker start windows
   ```

---

**That's it!** 🎉 With these steps, you've successfully set up Docker and created a **Windows 10** container in your Codespace.  

If you have any questions or need further assistance, feel free to ask. 🚀  

---

This file now reflects the changes we made to `docker-compose.yml`, including **credential management through the `.env` file** and the removal of unnecessary ports and configurations.  

Also, remember to **keep your sensitive environment variables secure** and **out of version control**. 🔒  

---

Let me know if you need any modifications! 🚀
