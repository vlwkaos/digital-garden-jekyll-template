---
title: 'WSL2에서 localhost사용'
author: 'vlwkaos'
published: true
---

WSL2에서 개발용 로컬 서버를 실행하고 윈도우에서 브라우저로 접속을 하면 안될 때가 있다. WSL2는 실행시 임의의 IP를 부여받기 떄문에 해당 IP로 윈도우 쪽에서 리다이렉팅이 필요하다. 원래는 자동으로 해줘야하는 기능이 맞지만 가끔 안되는 경우도 있고 만약 로컬 주소가 일반적으로 사용하는 localhost가 아닌 경우도 있는데, 윈도우 HOSTS 파일에 리다이렉팅을 추가해줌으로써 해결할 수 있다. 

아래 스크립트는 [이 블로그](https://abdus.dev/posts/fixing-wsl2-localhost-access-issue/)에서 가져왔고 하나의 문자열을 받던 것을 배열을 받도록 살짝 수정하였다.

```powershell
# 사용할 주소 문자열을 넣는다.
$hostnames = @("wsl")

# find ip of eth0
$ifconfig = (wsl -- ip -4 addr show eth0)
$ipPattern = "((\d+\.?){4})"
$ip = ([regex]"inet $ipPattern").Match($ifconfig).Groups[1].Value
if (-not $ip) {
    exit
}
Write-Host $ip

$hostsPath = "$env:windir/system32/drivers/etc/hosts"

$hosts = (Get-Content -Path $hostsPath -Raw -ErrorAction Ignore)
if ($null -eq $hosts) {
    $hosts = ""
}
$hosts = $hosts.Trim()

foreach($hostname in $hostnames) {
	# update or add wsl ip
	$find = "$ipPattern\s+$hostname"
	$entry = "$ip $hostname"

	if ($hosts -match $find) {
		$hosts = $hosts -replace $find, $entry
	}
	else {
		$hosts = "$hosts`n$entry".Trim()
	}

	try {
		$temp = "$hostsPath.new"
		New-Item -Path $temp -ItemType File -Force | Out-Null
		Set-Content -Path $temp $hosts

		Move-Item -Path $temp -Destination $hostsPath -Force
	}
	catch {
		Write-Error "cannot update wsl ip"
	}
}
```

이 파워쉘 스크립트는 HOSTS파일 마지막에 WSL2 인스턴스의 IP주소와 호스트 이름을 삽입한다. 예를 들어 위의 스크립트를 지금 그대로 돌리면 `123.123.123.123  wsl` 이라는 내용이 호스트 파일에 추가된다. 후에 브라우저 주소에 wsl을 입력해서 로컬 서버로 연결이 가능하다.

이를 자동화 하기 위해서 작업 스케쥴러를 이용할 수 있다. 

- 윈도우 이벤트 뷰어를 열고 (검색 > 이벤트 뷰어) WSL2 실행시 발생하는 네트워크 관련 이벤트를 찾아준다.

<p align='center'>
<img src='/assets/wsl2_localhost.png'/>
<br><span>이벤트 뷰어</span></p>

- 해당 이벤트를 우클릭하면 '이 이벤트에 작업연결'이라는 항목이 있는데 클릭한다. 
- 작업 설정 마법사가 열리면 프로그램 시작을 설정하는 부분까지 다음을 눌러 넘어간다.
- 프로그램에 `powershell.exe`를 입력하고 인자 입력칸에 `-file /위/스크립트/위치`를 기입하고 작업 생성을 마친다.
- 작업 스케쥴러를 열고 `이벤트 뷰어 작업` 폴더를 클릭하면 방금 생성한 작업이 보인다. 더블클릭하여 편집창을 열어준다.
- 일반 탭에서 '가장 높은 수준의 권한으로 실행'을 체크한다.
- 조건 탭에서 컴퓨터 AC 연결시에만 실행을 체크 해제한다.

이제 WSL이 실행될 때 자동으로 위의 스크립트를 실행할 수 있다.

