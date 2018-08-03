# 在Windows上面使用filezillaserver分享文件

## 环境说明
公司使用固定ip方式，由于笔者需要使用多个ip，所以，自己假设了路由器，设置本机ip为192.168.100.100，这就造成了公司同事跟笔者的主机不在一个网段。

## server配置
1. 添加用户并设置分享的文件目录
2. 设置passive mode settings
端口范围，建议5w-5.5w
external ip ：192.168.100.100
ftp over TLS settings: 生成crt证书，并完成设置

安装目录下面的配置文件内容：
```xml
<FileZillaServer>
    <Groups />
    <Users>
        <User Name="java">
            <Option Name="Pass">2D0AFC8B886D5F6CAB2F69231A4C55287D323C1D0321DF97B7CD091D33323DF8C18EC4CEF847F25D1455F807D5AE46F97B9EFBE143D9ECAF756D1F64868479D4</Option>
            <Option Name="Salt">)`L8&amp;LrcaSX%ZDM&quot;:wuLHfm[cm18`v}a^t&lt;rVM\nQ=4bN!@L0y&quot;v:s!u+1Ty2S/r</Option>
            <Option Name="Group" />
            <Option Name="Bypass server userlimit">0</Option>
            <Option Name="User Limit">0</Option>
            <Option Name="IP Limit">0</Option>
            <Option Name="Enabled">1</Option>
            <Option Name="Comments" />
            <Option Name="ForceSsl">0</Option>
            <IpFilter>
                <Disallowed />
                <Allowed />
            </IpFilter>
            <Permissions>
                <Permission Dir="D:\softwares_bak">
                    <Option Name="FileRead">1</Option>
                    <Option Name="FileWrite">0</Option>
                    <Option Name="FileDelete">0</Option>
                    <Option Name="FileAppend">0</Option>
                    <Option Name="DirCreate">0</Option>
                    <Option Name="DirDelete">0</Option>
                    <Option Name="DirList">1</Option>
                    <Option Name="DirSubdirs">1</Option>
                    <Option Name="IsHome">1</Option>
                    <Option Name="AutoCreate">0</Option>
                </Permission>
                <Permission Dir="E:\vedio">
                    <Aliases>
                        <Alias>/vedio</Alias>
                    </Aliases>
                    <Option Name="FileRead">1</Option>
                    <Option Name="FileWrite">0</Option>
                    <Option Name="FileDelete">0</Option>
                    <Option Name="FileAppend">0</Option>
                    <Option Name="DirCreate">0</Option>
                    <Option Name="DirDelete">0</Option>
                    <Option Name="DirList">1</Option>
                    <Option Name="DirSubdirs">1</Option>
                    <Option Name="IsHome">0</Option>
                    <Option Name="AutoCreate">0</Option>
                </Permission>
            </Permissions>
            <SpeedLimits DlType="1" DlLimit="10" ServerDlLimitBypass="0" UlType="0" UlLimit="10" ServerUlLimitBypass="0">
                <Download />
                <Upload />
            </SpeedLimits>
        </User>
    </Users>
    <Settings>
        <Item name="Serverports" type="string">21</Item>
        <Item name="Number of Threads" type="numeric">2</Item>
        <Item name="Maximum user count" type="numeric">0</Item>
        <Item name="Timeout" type="numeric">120</Item>
        <Item name="No Transfer Timeout" type="numeric">600</Item>
        <Item name="Check data connection IP" type="numeric">2</Item>
        <Item name="Service name" type="string"></Item>
        <Item name="Service display name" type="string"></Item>
        <Item name="Force TLS session resumption" type="numeric">1</Item>
        <Item name="Login Timeout" type="numeric">60</Item>
        <Item name="Show Pass in Log" type="numeric">0</Item>
        <Item name="Custom PASV IP type" type="numeric">1</Item>
        <Item name="Custom PASV IP" type="string">192.168.100.100</Item>
        <Item name="Custom PASV min port" type="numeric">50000</Item>
        <Item name="Custom PASV max port" type="numeric">51000</Item>
        <Item name="Initial Welcome Message" type="string">%v&#x0D;&#x0A;written by Tim Kosse (tim.kosse@filezilla-project.org)&#x0D;&#x0A;Please visit https://filezilla-project.org/</Item>
        <Item name="Admin port" type="numeric">14147</Item>
        <Item name="Admin Password" type="string"></Item>
        <Item name="Admin IP Bindings" type="string"></Item>
        <Item name="Admin IP Addresses" type="string"></Item>
        <Item name="Enable logging" type="numeric">0</Item>
        <Item name="Logsize limit" type="numeric">0</Item>
        <Item name="Logfile type" type="numeric">0</Item>
        <Item name="Logfile delete time" type="numeric">0</Item>
        <Item name="Disable IPv6" type="numeric">0</Item>
        <Item name="Enable HASH" type="numeric">0</Item>
        <Item name="Download Speedlimit Type" type="numeric">0</Item>
        <Item name="Upload Speedlimit Type" type="numeric">0</Item>
        <Item name="Download Speedlimit" type="numeric">10</Item>
        <Item name="Upload Speedlimit" type="numeric">10</Item>
        <Item name="Buffer Size" type="numeric">32768</Item>
        <Item name="Custom PASV IP server" type="string">http://ip.filezilla-project.org/ip.php</Item>
        <Item name="Use custom PASV ports" type="numeric">0</Item>
        <Item name="Mode Z Use" type="numeric">0</Item>
        <Item name="Mode Z min level" type="numeric">1</Item>
        <Item name="Mode Z max level" type="numeric">9</Item>
        <Item name="Mode Z allow local" type="numeric">0</Item>
        <Item name="Mode Z disallowed IPs" type="string"></Item>
        <Item name="IP Bindings" type="string">*</Item>
        <Item name="IP Filter Allowed" type="string"></Item>
        <Item name="IP Filter Disallowed" type="string"></Item>
        <Item name="Hide Welcome Message" type="numeric">0</Item>
        <Item name="Enable SSL" type="numeric">1</Item>
        <Item name="Allow explicit SSL" type="numeric">1</Item>
        <Item name="SSL Key file" type="string">C:\Users\northmeter\ftp\certificate.crt</Item>
        <Item name="SSL Certificate file" type="string">C:\Users\northmeter\ftp\certificate.crt</Item>
        <Item name="Implicit SSL ports" type="string">990</Item>
        <Item name="Force explicit SSL" type="numeric">0</Item>
        <Item name="Network Buffer Size" type="numeric">262144</Item>
        <Item name="Force PROT P" type="numeric">1</Item>
        <Item name="SSL Key Password" type="string">northmeter</Item>
        <Item name="Allow shared write" type="numeric">0</Item>
        <Item name="No External IP On Local" type="numeric">0</Item>
        <Item name="Active ignore local" type="numeric">1</Item>
        <Item name="Autoban enable" type="numeric">0</Item>
        <Item name="Autoban attempts" type="numeric">10</Item>
        <Item name="Autoban type" type="numeric">0</Item>
        <Item name="Autoban time" type="numeric">1</Item>
        <Item name="Minimum TLS version" type="numeric">0</Item>
        <SpeedLimits>
            <Download />
            <Upload />
        </SpeedLimits>
    </Settings>
</FileZillaServer>


```

## 设置路由器
1. DMZ主机将主机映射出去
2. 虚拟服务器将 14147的端口开放出去

## Windows系统设置
关闭防火墙