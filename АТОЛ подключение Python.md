https://integration.atol.ru/api/?python#diagnostika-soedineniya-s-ofd

```python
from libfptr10 import IFptr  
  
  
fptr = IFptr(r"C:\Program Files\ATOL\Drivers10\KKT\bin\fptr10.dll")  
version = fptr.version()  
  
  
settings = {  
    IFptr.LIBFPTR_SETTING_MODEL: IFptr.LIBFPTR_MODEL_ATOL_AUTO,  
    IFptr.LIBFPTR_SETTING_PORT: IFptr.LIBFPTR_PORT_COM,  
    IFptr.LIBFPTR_SETTING_COM_FILE: "COM7",  
    IFptr.LIBFPTR_SETTING_BAUDRATE: IFptr.LIBFPTR_PORT_BR_115200  
}  
fptr.setSettings(settings)  
  
fptr.open()  
fptr.setParam(IFptr.LIBFPTR_PARAM_REPORT_TYPE, IFptr.LIBFPTR_RT_OFD_TEST)  
fptr.report()

```

Печать Х-отчета
Если версия `x64` то путь `"C:\Program Files x 86\ATOL\Drivers10\KKT\bin\fptr10.dll"`