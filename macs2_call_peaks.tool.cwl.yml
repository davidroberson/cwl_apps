
#section 1/5 - Label
label: MACS2 Call Peaks

#section 2/5 - Inputs
inputs:
- id: bam
  type: File
  secondaryFiles:
  - pattern: .bai
    required: true
  inputBinding:
    position: 0a
    shellQuote: false
  sbg:fileTypes: BAM
- id: sort_order
  type: File
  inputBinding:
    position: 0
    shellQuote: false

#Section 3/5 - Base Command
baseCommand:
- bash
- call_peaks.sh

#section 4/5 - Requirements
requirements:
- class: ShellCommandRequirement
- class: DockerRequirement
  dockerPull: quay.io/jnasser/abc-container
- class: InitialWorkDirRequirement
  listing:
  - entryname: call_peaks.sh
    writable: false
    entry: |-
      macs2 callpeak -t $(inputs.bam.path) -n $(inputs.bam.nameroot).macs2 -f BAM -g hs -p .1 --call-summits --outdir ./

      #Sort narrowPeak file

      bedtools sort \
      -faidx $(inputs.sort_order.path) \
      -i $(inputs.bam.nameroot).macs2_peaks.narrowPeak \
      > $(inputs.bam.nameroot).macs2_peaks.narrowPeak.sorted
- class: InlineJavascriptRequirement

#section 5/5 - Outputs
outputs:
- id: macs2
  type: File[]?
  outputBinding:
    glob: '*.macs2*'

### below is boiler plate and rarely changes
cwlVersion: v1.2
class: CommandLineTool

$namespaces:
  sbg: https://sevenbridges.com
  
hints:
- class: sbg:SaveLogs
  value: '*.sh'  
