<img align="right" src="https://visitor-badge.laobi.icu/badge?page_id=noetovar5.serverinfoapp"/>
<p align="center">
  # serverinformationapp

noe tovar desing c# server information application
<p align="center">
  <a href="">
    <img src="https://github.com/noetovar5/portfolio/blob/main/server%20information.png?raw=true" height="60%" width="60%" alt="Youtube tutorial SQL Server Series"/>
 </a>
</p>



Set up environment in visual studio code




open a new termina in visual studio code and do the following.
1. create folder
2. open the form1.cs and copy this into it.
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\


using System;
using System.IO;
using System.Diagnostics;
using System.Windows.Forms;
using System.Management;
using System.Net;
using System.Net.NetworkInformation;

namespace ServerInfoApp
{
    public partial class Form1 : Form
    {
        private Label titleLabel;
        private Button infoButton;
        private Button diskInfoButton;
        private Button networkInfoButton;
        private Button printButton;
        private Button adminButton;
        private TextBox infoTextBox;
        private TextBox diskInfoTextBox;
        private TextBox networkInfoTextBox;

        public Form1()
        {
            InitializeComponent();
            this.Text = "Tovar Server Information App";
            this.StartPosition = FormStartPosition.CenterScreen;
            this.BackColor = System.Drawing.Color.DarkSlateGray; // This way the form stands out as not boring. Noe Tovar

            // Set the icon of the form
            //this.Icon = new System.Drawing.Icon("info.ico"); // Adjust the path to your icon file

            // Initialize and configure the title label
            titleLabel = new Label();
            titleLabel.Text = "Tovar Server Information App";
            titleLabel.Font = new System.Drawing.Font(titleLabel.Font.FontFamily, 18, System.Drawing.FontStyle.Bold);
            titleLabel.ForeColor = System.Drawing.Color.White;
            titleLabel.AutoSize = true;
            titleLabel.TextAlign = System.Drawing.ContentAlignment.MiddleCenter;
            titleLabel.Anchor = AnchorStyles.Top;
            titleLabel.Location = new System.Drawing.Point((this.ClientSize.Width - titleLabel.Width) / 2, 10);
            titleLabel.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;

            // Initialize and configure the server info button
            infoButton = CreateStyledButton("Get Server Information", 50, 50);
            infoButton.Click += new EventHandler(InfoButton_Click);

            // Initialize and configure the disk info button
            diskInfoButton = CreateStyledButton("Get Disk Information", 50, 150);
            diskInfoButton.Click += new EventHandler(DiskInfoButton_Click);

            // Initialize and configure the network info button
            networkInfoButton = CreateStyledButton("Get Network Information", 50, 250);
            networkInfoButton.Click += new EventHandler(NetworkInfoButton_Click);

            // Initialize and configure the server info textbox
            infoTextBox = new TextBox();
            infoTextBox.Multiline = true;
            infoTextBox.ScrollBars = ScrollBars.Vertical;
            infoTextBox.Size = new System.Drawing.Size(400, 100);
            infoTextBox.Location = new System.Drawing.Point(300, 50);
            infoTextBox.Anchor = AnchorStyles.Top;

            // Initialize and configure the disk info textbox
            diskInfoTextBox = new TextBox();
            diskInfoTextBox.Multiline = true;
            diskInfoTextBox.ScrollBars = ScrollBars.Vertical;
            diskInfoTextBox.Size = new System.Drawing.Size(400, 100);
            diskInfoTextBox.Location = new System.Drawing.Point(300, 150);
            diskInfoTextBox.Anchor = AnchorStyles.Top;

            // Initialize and configure the network info textbox
            networkInfoTextBox = new TextBox();
            networkInfoTextBox.Multiline = true;
            networkInfoTextBox.ScrollBars = ScrollBars.Vertical;
            networkInfoTextBox.Size = new System.Drawing.Size(400, 100);
            networkInfoTextBox.Location = new System.Drawing.Point(300, 250);
            networkInfoTextBox.Anchor = AnchorStyles.Top;

            // Initialize and configure the print button
            printButton = CreateStyledButton("Print", 300, 370); // Centered at the bottom
            printButton.Size = new System.Drawing.Size(100, 40);
            printButton.Anchor = AnchorStyles.Bottom;
            printButton.Click += new EventHandler(PrintButton_Click);

            // Initialize and configure the admin button
            adminButton = CreateStyledButton("Admin", 500, 370); // Positioned to the right of the Print button
            adminButton.Size = new System.Drawing.Size(100, 40);
            adminButton.Anchor = AnchorStyles.Bottom;
            adminButton.Click += new EventHandler(AdminButton_Click);

            // Add controls to the form
            this.Controls.Add(titleLabel);
            this.Controls.Add(infoButton);
            this.Controls.Add(diskInfoButton);
            this.Controls.Add(networkInfoButton);
            this.Controls.Add(infoTextBox);
            this.Controls.Add(diskInfoTextBox);
            this.Controls.Add(networkInfoTextBox);
            this.Controls.Add(printButton);
            this.Controls.Add(adminButton);

            // Center the title after the form and controls are fully initialized
            this.Load += new EventHandler(CenterTitleLabel);
        }

        private Button CreateStyledButton(string text, int x, int y)
        {
            Button button = new Button();
            button.Text = text;
            button.Font = new System.Drawing.Font(button.Font, System.Drawing.FontStyle.Bold);
            button.Size = new System.Drawing.Size(200, 50);
            button.Location = new System.Drawing.Point(x, y);
            button.FlatStyle = FlatStyle.Flat;
            button.FlatAppearance.BorderColor = System.Drawing.Color.Black;
            button.FlatAppearance.BorderSize = 2;
            button.BackColor = System.Drawing.Color.MintCream;
            return button;
        }

        private void CenterTitleLabel(object sender, EventArgs e)
        {
            titleLabel.Left = (this.ClientSize.Width - titleLabel.Width) / 2;
        }

        private void InfoButton_Click(object sender, EventArgs e)
        {
            string cpuInfo = GetCPUInfo();
            string ramInfo = GetRAMInfo();
            string osInfo = GetOSInfo();
            string hostName = Environment.MachineName;
            string userName = Environment.UserName;
            string domainName = Environment.UserDomainName;
            string productKey = GetWindowsProductKey();

            // Display each type of information on a new line
            infoTextBox.Text = $"**Host Name:** {hostName}\n\n" +
                               $"**User Name:** {userName}\n\n" +
                               $"**Domain Name:** {domainName}\n\n" +
                               $"**CPU:**\n{cpuInfo}\n\n" +
                               $"**RAM:**\n{ramInfo}\n\n" +
                               $"**Operating System:**\n{osInfo}\n\n" +
                               $"**Product Key:** {productKey}";
        }

        private void DiskInfoButton_Click(object sender, EventArgs e)
        {
            string diskInfo = GetDiskInfo();

            // Display the disk information with each data point on its own line, with whitespace for readability
            diskInfoTextBox.Text = diskInfo;
        }

        private void NetworkInfoButton_Click(object sender, EventArgs e)
        {
            string networkInfo = GetNetworkInfo();

            networkInfoTextBox.Text = networkInfo;
        }

        private void PrintButton_Click(object sender, EventArgs e)
        {
            string output = "Server Information:\n" + infoTextBox.Text +
                            "\n\nDisk Information:\n" + diskInfoTextBox.Text +
                            "\n\nNetwork Information:\n" + networkInfoTextBox.Text +
                            "\n\nFor more information about this application, contact Noe Tovar at https://www.linkedin.com/in/noe-tovar-mba";

            File.WriteAllText("ServerInformation.txt", output);
            MessageBox.Show("Information saved to ServerInformation.txt");
        }

        private void AdminButton_Click(object sender, EventArgs e)
        {
            Process.Start(new ProcessStartInfo("https://drive.google.com/drive/folders/15LfyISKWOLvk6MKC2RYTjs2MeNYAng2s?usp=sharing") { UseShellExecute = true });
        }

        private string GetCPUInfo()
        {
            string cpuBaseSpeed = "";
            string cpuCores = "";
            string logicalProcessors = "";

            ManagementObjectSearcher searcher = new ManagementObjectSearcher("select * from Win32_Processor");
            foreach (ManagementObject obj in searcher.Get())
            {
                cpuBaseSpeed = $"{obj["MaxClockSpeed"]} MHz";
                cpuCores = obj["NumberOfCores"].ToString();
                logicalProcessors = obj["NumberOfLogicalProcessors"].ToString();
            }

            return $"Base Speed: {cpuBaseSpeed}\nCores: {cpuCores}\nLogical Processors: {logicalProcessors}";
        }

        private string GetRAMInfo()
        {
            string ram = "";
            ManagementObjectSearcher searcher = new ManagementObjectSearcher("select * from Win32_ComputerSystem");
            foreach (ManagementObject obj in searcher.Get())
            {
                double totalRam = Math.Round(Convert.ToDouble(obj["TotalPhysicalMemory"]) / (1024 * 1024 * 1024), 2);
                ram = $"{totalRam} GB";
            }

            return ram;
        }

        private string GetOSInfo()
        {
            string osName = "";
            string osVersion = "";
            ManagementObjectSearcher searcher = new ManagementObjectSearcher("select * from Win32_OperatingSystem");
            foreach (ManagementObject obj in searcher.Get())
            {
                osName = obj["Caption"].ToString();
                osVersion = obj["Version"].ToString();
            }

            return $"{osName} (Version: {osVersion})";
        }

        private string GetDiskInfo()
        {
            string diskInfo = "";
            ManagementObjectSearcher searcher = new ManagementObjectSearcher("select * from Win32_LogicalDisk");
            foreach (ManagementObject obj in searcher.Get())
            {
                string volumeName = obj["VolumeName"]?.ToString() ?? "Unnamed Volume";
                string volumeType = obj["Description"].ToString();
                double totalSize = Math.Round(Convert.ToDouble(obj["Size"]) / (1024 * 1024 * 1024), 2);
                double freeSpace = Math.Round(Convert.ToDouble(obj["FreeSpace"]) / (1024 * 1024 * 1024), 2);
                double percentFree = Math.Round((freeSpace / totalSize) * 100, 2);

                diskInfo += $"**Volume:** {volumeName}\n" +
                            $"**Type:** {volumeType}\n" +
                            $"**Total Size:** {totalSize} GB\n" +
                            $"**Free Space:** {freeSpace} GB\n" +
                            $"**Percent Free:** {percentFree}%\n\n";
            }

            return diskInfo.TrimEnd('\n');
        }

        private string GetNetworkInfo()
        {
            string ipAddress = "";
            string subnetMask = "";
            string gateway = "";
            string dnsServers = "";
            string uuid = "";

            foreach (NetworkInterface ni in NetworkInterface.GetAllNetworkInterfaces())
            {
                if (ni.OperationalStatus == OperationalStatus.Up)
                {
                    foreach (UnicastIPAddressInformation ip in ni.GetIPProperties().UnicastAddresses)
                    {
                        if (ip.Address.AddressFamily == System.Net.Sockets.AddressFamily.InterNetwork)
                        {
                            ipAddress = ip.Address.ToString();
                            subnetMask = ip.IPv4Mask.ToString();
                            break;
                        }
                    }

                    foreach (GatewayIPAddressInformation gatewayAddr in ni.GetIPProperties().GatewayAddresses)
                    {
                        gateway = gatewayAddr.Address.ToString();
                    }

                    // Corrected code: Iterate over the DnsAddresses collection
                    foreach (IPAddress dns in ni.GetIPProperties().DnsAddresses)
                    {
                        if (dns.AddressFamily == System.Net.Sockets.AddressFamily.InterNetwork)
                        {
                            dnsServers += $"{dns}\n";
                        }
                    }

                    uuid = ni.Id;
                    break;
                }
            }

            return $"**IP Address:** {ipAddress}\n" +
                   $"**Subnet Mask:** {subnetMask}\n" +
                   $"**Gateway:** {gateway}\n" +
                   $"**DNS Servers:**\n{dnsServers.TrimEnd('\n')}\n" +
                   $"**UUID:** {uuid}";
        }

        private string GetWindowsProductKey()
        {
            try
            {
                ManagementObjectSearcher searcher = new ManagementObjectSearcher("select * from SoftwareLicensingService");
                foreach (ManagementObject obj in searcher.Get())
                {
                    return obj["OA3xOriginalProductKey"]?.ToString() ?? "Product Key not found";
                }
                return "Product Key not found";
            }
            catch (Exception ex)
            {
                return $"Error retrieving Product Key: {ex.Message}";
            }
        }
    }
}




### **Step 4: Build and Run the Application**

#### 1. **Build the Application**
   - In the terminal, build the application by running:
     ```b
     ash
     dotnet build
     ```

#### 2. **Run the Application**
   - Run the application by executing:
     ```bash
     dotnet run
     ```

### **Step 5: Test the Application**
   - A window should appear with a button labeled "Get Server Information".
   - When you click the button, the application will display the CPU base speed, the number of CPU cores, the number of logical processors, the total RAM, and the operating system information in the text box below.
######Finally you want to create a folder named Resources and add the icon into that folder 'filename.ico'



then build it as an .exe file

\\\\\\\\\\\\\\\\\\
replace the content of serverinfoapp.csproj file
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net8.0-windows</TargetFramework>
    <Nullable>enable</Nullable>
    <UseWindowsForms>true</UseWindowsForms>
    <ImplicitUsings>enable</ImplicitUsings>
    <ApplicationIcon>Resources\info.ico</ApplicationIcon>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Management" Version="8.0.0" />
  </ItemGroup>

</Project>
