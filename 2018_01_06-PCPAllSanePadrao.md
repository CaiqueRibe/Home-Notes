---
tags:
  - Padroes
  - Desenvolvimento
  - GMAF
  - VB6
---
````
    Padrao de Objetos - PCP All Sane
``````
# Posicionamento
### Fichario / SSTab
- Left - 120
- Top - 120


### Frame / FraFundo(0)
- Left - 180
- Top - 420  

# Spacing entre TextBoxes
- AverageHeight --> 330
- SpaceBetween --> 60
- EspacoTotal --> 380

# Font
| Objeto | Font | Tipo     | Size    |   
| ---- | :---:| :------:   | :--:   |
| Fichario | Arial | Regular  | 9  |
| Frame | Arial | Regular | 9  |
| Label | Arial | Regular | 9  |
| TextBox | Courier New | Regular | 9 |
| CombotBox | Courier New | Regular | 9  |

# `Eventos Padroes`
## Form   (4 events)
- Load() --> Initiate needed stuff, set to barraBotoes 0 after set up
- Activate() --> Ativa buttons at BarraFuncoes 
- Resize --> Redimensiona o fichario
- Unload() --> Verifys that there isent any change on screen, after closing reabilita tab da tela
--------------
## SSTab (4 events)
- GotFocus() --> f_pronto
- Click() --> Deactivate not needed forms and habilitar one in focus 
- KeyPress() -->  If KeyAscii = vbKeyReturn Then SendKeys "{TAB}"
- KeyUp() --> gfHelp gfHelp KeyCode, Me.Name, ActiveControl.Name
## TextBox (2/5 events)
- **KeyUp()** --> gfHelp  gfHelp KeyCode, Me.Name, ActiveControl.Name
- **GotFocus()** --> f_mh or f_pronto (display text on barra de pe)
- LostFocus() --> auto preenchimento de campos apartir da info recebida da pesquisa do CMD 
- KeyDown() --> F4 de Pesquisa - forcar cmd_click  "If KeyCode = vbKeyF4 And cmdNacao.Enabled Then cmdNacao_Click"
- Change() --> clears up realted fields that are preenchidos com LostFocus()
#### CMD (1 event)
- Click() --> TelaDePesquisaDados
#### TextBox(Descricao/RightSide) (2 events)
- GotFocus() --> f_pronto
- KeyUp() --> gfHelp  gfHelp KeyCode, Me.Name, ActiveControl.Name
-------
## TGBDGrid (5 events)
- **HeadClick()** --> Quicksorts the selected XarrayPesquisa and rebinds/refreshes grade
- **DoubleClick()** --> If item clicked on exists and is found, its selected and focused on in the front tab
- GotFocus() --> 
- KeyPress() --> 
- KeyUP() --> gfHelp  "gfHelp KeyCode, Me.Name, ActiveControl.Name"
## General (7 events)
- BarraBotoes
- fw_reg_alteracao --> Verifica si houve alteracao na tela como um todo, if yes (i_b_dig = true)
- fw_reg_pre_gravacao --> Verifica preenchimento necessario da tela para que seja possivel a tentativa de gravacao
- **MontaGradePesquisa** --> Selects all data from the given table, filters it com base na filtragem feita, then definirGrade
- **msGravadados**
- **msMostrarDados**
- **msLimparDados**
------------
## TDBGrid (12 events)
- tdbgResistencias_AfterColUpdate --> Change RegInserido To 1 / gfPesquisa in columns with button to update the upnext column (Examples - Ferramenta, Maquina, Molde)
- tdbgResistencias_AfterDelete --> mblnDigiGrade = True / Signalizes that there has been change  
- tdbgResistencias_BeforeDelete --> if sequencial da linha NOT = "" (Meaning it hasent been inclued yet) / 
  mstrFerramentasExclusao = mstrFerramentasExclusao & Format(tdbgFerramentas.Columns(mtypColunasFerramentas.Sequencial), "0000000000") & "/"  
- BeforeColEdit --> cleans columns that preenche outras (Ferramenta, Maquina, Molde) / tdbgFerramentas.Columns(mtypColunasFerramentas.Descricao) = ""
  - ButtonClick --> TelaPesquisaDados
- tdbgResistencias_Click --> If Not ActiveControl.Name = tdbgFerramentas.Name Then tdbgFerramentas.SetFocus
- tdbgResistencias_Error --> Response = False
  - GotFocus --> tdbgFerramentas_RowColChange 0,0
- tdbgResistencias_KeyDown --> vbKeyF4 check for ButtonClick sends for needed columns / sendkeys {Down} {Home} vbKeyReturn for needed columns
- KeyPress --> f_numerico and what not for needed columns
- tdbgResistencias_KeyUp --> gfHelp KeyCode, Me.Name, ActiveControl.Name
- tdbgResistencias_RowColChange --> f_pronto / f_mh
------------
``````````vbScript
' BarraBotoes and the case which will be activated by Parameter

Public Sub BarraBotoes(Button As Integer)
   Select Case Button
      Case "0" 'Primeiro...
      Case "1" 'Anterior...
      Case "2" 'Proximo...
      Case "3" 'Ultimo...
      Case "4" 'Novo...
         LimpaDados
      Case "5" 'Excluir...
         DeletaDados
      Case "6" 'Gravar...
         GravaDados
      Case "8" 'Refresh...
         AtualizaDados
      Case "9" 'Pesquisa...
         Fichario.Tab = 0
      Case "10" 'Relatoriodf
    End Select
End sub


'Primeiro'
 gstrSQL = "SELECT TOP 1 * " & _
           "FROM CE_Grupo " & _
           "ORDER BY Grupo"
------------------------------------------------------------------------
'Anterior'
 gstrSQL = "SELECT TOP 1 * " & _
           "FROM CE_Grupo " & _
           "WHERE Grupo < " & ToSQLInteger(tdbnGrupo) & " " & _
           "ORDER BY Grupo DESC"
------------------------------------------------------------------------
'Proximo'
gstrSQL = "SELECT TOP 1 * " & _
          "FROM CE_Grupo " & _
          "WHERE Grupo > " & ToSQLInteger(tdbnGrupo) & " " & _
          "ORDER BY Grupo"
------------------------------------------------------------------------
'Ultimo'
gstrSQL = "SELECT TOP 1 * " & _
          "FROM CE_Grupo " & _
     	  "ORDER BY Grupo DESC"
------------------------------------------------------------------------
'Delete'
gstrSQL = "DELETE " & _
          "FROM CE_Grupo " & _
          "WHERE Grupo = " & ToSQLInteger(tdbnGrupo)
------------------------------------------------------------------------
'Refresh'
gstrSQL = "SELECT * " & _
          "FROM CE_Grupo " & _
          "WHERE Grupo = " & ToSQLInteger(tdbnGrupo)
``````````````````````