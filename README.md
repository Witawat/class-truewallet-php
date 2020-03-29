# Truemoney Wallet Class
- สำคัญมากๆจำเป็นต้อง Cronjob มาที่ access_token.php ตลอดเวลา เพื่อทำให้ access_token ไม่ตาย !

# Variable
- $phone    // เบอร์ทรูมันนี่วอลเลท
- $password // รหัสผ่านทรูมันนี่วอลเลท
- $otp_code // OTP ใน SMS 6 หลัก (ตัวเลข)
- $otp_ref  // OTP ใน SMS REF 4 หลัก (ตัวอักษร)
- $access_token // ได้จาก SubmitLoginOTP

# Example Truemoney Wallet Class
```php
<?php
  require "Truewallet.php";
  $tw = new TrueWalletClass($phone, $password);
  
  print_r($tw->RequestLoginOTP());
  print_r($tw->SubmitLoginOTP($otp_code, $phone, $otp_ref));
  
  $tw->setAccessToken($access_token);
  $data = $tw->GetTransaction();
  foreach ($data["data"]["activities"] as $transfer) {
    $values = $tw->GetTransactionReport($transfer["report_id"]);
    print_r($values);
  }
?>
```
# Check Get Money
#### หากต้องการเช็คว่ารายการได้เป็นการโอนผ่าน ทรูมันนี่วอลเลท ให้ใช้ If
- $transfer["original_type"] == "transfer" //โอนผ่านเเอพทรูมันนี่วอลเลท
- $transfer["original_action"] == "creditor" //เช็คว่าเป็นรายการโอนเข้ารายการที่เราโอนออกจากบัญชีจะไม่เเสดงครับ

# GetTransaction Value
- $transfer_id //เลขอ้างอิง
- $money //จำนวนเงิน
- $sender_phone //เบอร์คนที่โอนมา

```php
$transfer_id	= $values["data"]["section4"]["column2"]["cell1"]["value"];
$money = $values["data"]["section3"]["column1"]["cell1"]["value"];
$sender_phone = $values["data"]["section2"]["column1"]["cell1"]["value"];
```

# Example Cronjob Windows
- เเก้ไข Url [ตรงนี้](https://github.com/ekkamon/class-truewallet-php/blob/master/CronjobAPIWallet/Program.cs) บรรทัด: 17
- เเก้ไข Loop Time [ตรงนี้](https://github.com/ekkamon/class-truewallet-php/blob/master/CronjobAPIWallet/Program.cs) บรรทัด: 39 // 1000 = วินาที

#### access_token.php
- ใส่ข้อมูล $phone
- ใส่ข้อมูล $password
- ใส่ข้อมูล $access_token
##### นำไฟล์อัพลง xampp, apache เเละ เอาลิ้งค์มาใส่ใน Cronjob 

# Help my Truemoney Wallet Class .-.
- โครง Class ตัวนี้นำมาจาก [likecyber](https://github.com/likecyber)
- Parameters เเละ URL ตัวนี้นำมาจาก [chick4nnn](https://gist.github.com/chick4nnn/0b070c673f7977c42db5fd499055d28f)
- เเนวทางเลี้ยง access_token ตัวนี้นำมาจาก [Manatchai Roengtum](https://www.facebook.com/iceyboy.kung)
