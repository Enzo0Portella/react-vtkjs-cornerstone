# Formatos de Imagem no React VTK.js Viewport

## Visão Geral

O React VTK.js Viewport é um componente para visualização de imagens médicas em 3D, utilizando VTK.js como motor de renderização. Este componente permite a visualização e manipulação de imagens médicas em formato DICOM dentro de aplicações React, incluindo visualizações Multi-Planar Reconstruction (MPR) e renderização volumétrica.

## Formatos de Imagem Suportados

### Formato Principal: DICOM
- **Descrição**: DICOM (Digital Imaging and Communications in Medicine) é o padrão internacional para imagens médicas e informações relacionadas.
- **Extensão**: .dcm
- **Características**:
  - Contém metadados detalhados do paciente e do estudo
  - Suporta várias modalidades (CT, MRI, PET, etc.)
  - Possibilita armazenamento de múltiplos frames em um único arquivo

### Suporte para Imagens:
- **Bits Alocados**: Atualmente suporta principalmente imagens de 16 bits
- **Limitações**:
  - Imagens de 8 bits (assinadas ou não) ainda não são suportadas
  - Imagens multicomponentes (como RGB) não são suportadas

## Protocolos de Acesso

### DICOMweb
O componente utiliza o protocolo DICOMweb para acessar imagens de servidores PACS:

- **WADO-RS** (Web Access to DICOM Objects by RESTful Services):
  - Usado para recuperar imagens DICOM através de requisições HTTP
  - Formato de URL: `wadors:[servidor]/studies/[studyUID]/series/[seriesUID]/instances/[instanceUID]/frames/[frameNumber]`

- **Cliente**: A biblioteca utiliza `dicomweb-client` para comunicação com servidores DICOMweb

### Carregadores de Imagem

O projeto utiliza o ecossistema Cornerstone para carregamento e processamento inicial das imagens:

- **cornerstone-wado-image-loader**: Carrega imagens DICOM de servidores DICOMweb
- **dicom-parser**: Analisa arquivos DICOM e extrai metadados
- **cornerstone-core**: Fornece funcionalidades básicas para renderização de imagens médicas

## Processamento e Conversão

O fluxo de processamento de imagens inclui:

1. Carregamento de imagens DICOM via `cornerstone-wado-image-loader`
2. Extração de metadados e dados de pixel
3. Conversão para formato VTK.js `vtkImageData` usando:
   - `vtkDataArray` para armazenar valores de pixels
   - Conversão para `Float32Array` independente do formato original
4. Aplicação de transformações espaciais baseadas em metadados DICOM:
   - Orientação da imagem (ImageOrientationPatient)
   - Espaçamento de pixels
   - Posição da imagem

## Fontes de Dados

As imagens podem ser obtidas de várias fontes:

1. **Servidores DICOMweb**:
   - Exemplo usado nos exemplos: `https://server.dcmjs.org/dcm4chee-arc/aets/DCM4CHEE/rs`
   - Outros servidores PACS com suporte a DICOMweb

2. **Arquivos locais**:
   - Pode ser integrado com sistemas de arquivos locais em aplicações Electron
   - Suporte potencial para carregamento direto de arquivos DICOM através de APIs de arquivo do navegador

3. **Formato VTI**:
   - Alguns exemplos também demonstram carregamento de arquivos VTI (formato nativo do VTK)
   - Útil para datasets não-DICOM ou pré-processados

## Capacidades de Renderização

O componente oferece várias capacidades de visualização:

1. **Visualização 2D (MPR)**:
   - Planos axial, sagital e coronal
   - Suporte para crosshairs (linhas de referência cruzadas)
   - Rotação dos planos de visualização

2. **Renderização Volumétrica 3D**:
   - Mapeamento de opacidade e transferência de cor
   - Presets para diferentes modalidades (CT, MR, etc.)
   - Fusão de volumes (ex: PET-CT)

3. **Capacidades Interativas**:
   - Nivelamento de janela (Window/Level)
   - Rotação e zoom
   - Crosshairs ajustáveis
   - Ferramentas de pintura/segmentação

## Modalidades Suportadas

O componente tem suporte especial para várias modalidades de imagem:

- **CT (Tomografia Computadorizada)**
- **MR (Ressonância Magnética)**
- **PET (Tomografia por Emissão de Pósitrons)**
- **Fusão PET-CT**

Cada modalidade possui presets específicos para renderização volumétrica otimizada.
