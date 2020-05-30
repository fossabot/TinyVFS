# TinyVFS

[![nuget](https://img.shields.io/nuget/v/TinyVFS.svg?style=flat-square)](https://www.nuget.org/packages/TinyVFS) [![stats](https://img.shields.io/nuget/dt/TinyVFS.svg?style=flat-square)](https://www.nuget.org/stats/packages/TinyVFS?groupby=Version) [![GitHub license](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://raw.githubusercontent.com/hueifeng/TinyVFS/master/LICENSE)

`TinyVFS` ��һ�������ļ�ϵͳ����ABP vNext��ܵ������������Խ�js��css��image��cshtml���ļ�Ƕ�뵽�����У�
��������ʱ���Խ������������ļ�һ��ȥʹ�á�

#### �ص�

* �ڵ���Ӧ���У������Խ�ǰ�˺ͺ�̨������ϵͳ���ֵ�������Ŀ������
* �ڿ������������ÿ�����Աͬʱ���п�����ͬ��ҵ�����ģ��
* �����������ǽ�ϵͳ����ģ���ֺ���װ��һ��

## ��������

1��ͨ��Nuget��װ���

```
Install-Package TinyVFS
```

2��ע��Ƕ���ļ�

�༭web��Դ��Ŀ`.csproj`
```
<ItemGroup>
  <EmbeddedResource Include="MyResources\**\*.*" />
  <Content Remove="MyResources\**\*.*" />
</ItemGroup>
```

ͨ�����´���Ƭ�ν��ļ�Ƕ�뵽�����ļ�ϵͳ��

```csharp
     services.AddVirtualFilesService();
            services.Configure<VirtualFileSystemOptions>(options =>
            {
                options.FileSets.AddEmbedded<WebApplication1.Pages.IndexModel>("WebResources");
            });
```



3����ȡ�����ļ�

Ƕ�뵽���򼯺��ͨ��`IVirtualFileProvider`����ȡ�ļ�����Ŀ¼����

```csharp
public class MyService
{
    private readonly IVirtualFileProvider _virtualFileProvider;

    public MyService(IVirtualFileProvider virtualFileProvider)
    {
        _virtualFileProvider = virtualFileProvider;
    }

    public void Foo()
    {
        //Getting a single file
        var file = _virtualFileProvider.GetFileInfo("/MyResources/js/test.js");
        var fileContent = file.ReadAsString(); //ReadAsString is an extension method of ABP

        //Getting all files/directories under a directory
        var directoryContents = _virtualFileProvider.GetDirectoryContents("/MyResources/js");
    }
}
```

4����̬�����ļ�

�������ڱ������п���ʱ��Ҳ�����ǻ����Դ��Ŀ�еľ�̬�ļ������޸ģ���ô�����������������ȥ�������ɴ���.....

�������ǿ���ͨ��`ReplaceEmbeddedByPhysical `��ͨ�������ˢ�¼��ɻ�ȡ���µ��ļ���Ϣ

```csharp
  services.AddVirtualFilesService();
            services.Configure<VirtualFileSystemOptions>(options =>
            {
                options.FileSets.ReplaceEmbeddedByPhysical<WebApplication1.Pages.IndexModel>(
                 Path.Combine(WebHostEnvironment.ContentRootPath, "..\\WebResources")
             );
            });

```

5�������ļ��м��

�����ļ��м��������ͻ���/������ṩǶ��ʽ(js, css, image ...)�ļ�, 
���� wwwroot �ļ����е�����(��̬)�ļ�һ��. �ھ�̬�ļ��м��֮�������, ������ʾ:

```csharp
app.UseVirtualFiles();
```

�������չ�����ļ���ʽ��ô����ʹ�����ط�����������ʾ:

```csharp
var provider = new FileExtensionContentTypeProvider();
provider.Mappings[".less"] = "text/css";
app.UseVirtualFiles(provider);
```


ͨ�����������ļ��м����ʹ�������ļ���ͬ��λ�÷��������ļ����Ӷ�ʹ�����ļ����������ļ���Ϊ���ܡ�

6��ASP.NET Core����

�����ļ�����ֱ�Ӽ��ɵ�ASP.NET Core��

�� �����ļ�������WebӦ�ó����е�����̬�ļ�һ��ʹ�á�
�� Razor Views, Razor Pages, js, css, ͼ���ļ�����������Web���ݿ���Ƕ�뵽�����в��������ļ�һ��ʹ�á�
�� Ӧ�ó�����Ը���ģ�飨web��Դ���������ļ�, ���񽫾�����ͬ���ƺ���չ�����ļ����������ļ���ͬһ�ļ�����һ��.

7��Views & Pages

���ǲ���Ҫ�κ����ã���������Ӧ�ó�����ʹ�ã�����������Ŀ¼������Щ�ļ�ʱ���򸲸������ļ���


## ����

��������뷨���Լ�����������߷��ֱ���Ŀ������Ҫ�Ľ��Ĵ��룬��ӭFork���ύPR��


## �ο�

- [ABP vNext](https://github.com/abpframework/abp)
