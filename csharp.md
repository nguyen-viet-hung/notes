Notes về C# trên Linux

Lệnh:

#dotnet restore: Sẽ check các framework và các dependancies packages của NuGet và down về thư mực ~/.nuget
Do đó, mỗi khi thay đổi các dependancies hay framework, chúng ta cần chạy lại lệnh này để get/update

#dotnet build: Sẽ build project
#dotnet run: Sẽ thực thi

-------------------------------------------------------------------------------------------------------
Khi build mà phát hiện lỗi không resolve đươc 1 namespace hay 1 class, cần check lại trên NuGet xem là
là thuộc package gì, rồi thêm vào mục "dependancies" trong file project.json và restore lại.
-------------------------------------------------------------------------------------------------------
Chú ý: Trên linux, mỗi user sẽ có 1 workspace riêng để chứa các packages này ngoài phần core cung cấp
nằm trong 2 thư mục : ~/.dotnet và ~/.nuget