FROM mcr.microsoft.com/windows/servercore:ltsc2019
WORKDIR /tmp

RUN net user /add custom-admin
RUN net localgroup Administrators custom-admin /add
USER custom-admin

RUN powershell -Command " \
    $url = \"https://repo.continuum.io/miniconda/Miniconda3-latest-Windows-x86_64.exe\"; \
    $client = new-object System.Net.WebClient; \
    $client.DownloadFile( $url, \"miniconda3.exe\"); \
    ./miniconda3.exe /S /D=C:\dev\bin\anaconda | Write-Output; \
    del miniconda3.exe; \
    "
RUN setx path "%path%;C:\dev\bin\anaconda;C:\dev\bin\anaconda\Library\mingw-w64\bin;C:\dev\bin\anaconda\Library\usr\bin;C:\dev\bin\anaconda\Library\bin;C:\dev\bin\anaconda\Scripts;"

RUN powershell -Command " \
    conda clean --all -y;

RUN powershell -Command " \
    $url = \"http://go.microsoft.com/fwlink/?LinkId=691126\"; \
    $client = new-object System.Net.WebClient; \
    $client.DownloadFile( $url, \"msvc_build_tools.exe\"); \
    ./msvc_build_tools.exe /full /q | Write-Output; \
    Start-Sleep -s 20; \
    del msvc_build_tools.exe; \
    "

RUN powershell -Command " \
    $url = \"http://www.7-zip.org/a/7z1514-x64.exe\"; \
    $client = new-object System.Net.WebClient; \
    $client.DownloadFile( $url, \"7zip_15_14.exe\"); \
    ./7zip_15_14.exe /S | Write-Output; \
    del 7zip_15_14.exe; \
    "

RUN powershell -Command " \
    $url = \"http://www.dependencywalker.com/depends22_x64.zip\"; \
    $client = new-object System.Net.WebClient; \
    $client.DownloadFile( $url, \"dependency_walker.zip\"); \
    & \"C:/Program Files/7-zip/7z.exe\" x dependency_walker.zip \"*.*\" -oc:\tools\dependency_walker -y -r | Write-Output; \
    del dependency_walker.zip; \
    "

RUN powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))"

RUN setx path "%path%;C:\ProgramData\chocolatey\bin;C:\Program\ Files\7-zip;C:\tools\dependency_walker"

RUN conda config --set always_yes true && conda config --set anaconda_upload false

RUN chocolatey feature enable -n allowGlobalConfirmation

RUN powershell -Command " \
    conda install conda-build git svn anaconda-client anaconda-build; \
    conda clean --all; \
    "