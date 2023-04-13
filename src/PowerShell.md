##### 1. Read specific lines from the text contents of a file

- Select specific, individual lines:
  ```ps1
  Get-Content .\Web.config | Select-Object -Index 80, 81
  ```
- Select five lines after skipping the first forty lines (select lines 41 to 45):
  ```ps1
  Get-Content .\Web.config | Select-Object -Skip 40 -First 5
  ```
- Select a range of lines:

  (Select a single range of lines)
  ```ps1
  Get-Content .\Web.config | Select-Object -Index (15..24)
  ```

  (Select multiple ranges of lines)
  ```ps1
  Get-Content .\Web.config | Select-Object -Index (6..6+30..42+60)
  ```

##### 2. Generate 10000 dummy files with equal length indices in filenames

- Using .NET's `ToString()` method:
  ```ps1
  1..10000 | % tostring 00000 | % { Write-Output "Sample data for test - $_" > "Test_Data_$_.txt" }
  ```

- Typecasting & using .NET's `String.PadLeft()` method:
  ```ps1
  1..10000 | % { Write-Output "Sample data for test - $(([string]$_).PadLeft(5,"0"))" > "Test_Data_$(([string]$_).PadLeft(5,"0")).txt" }
  ```
