Use DFClient.pkg
Use DFUnit\Reporting\ReporterManager.pkg
Use DFUnit\Reporting\Reporters\UIListReporter.pkg

{ Visibility=Private }
Define DFUNIT_UI_WINDOW_HEIGHT for 255
{ Visibility=Private }
Define DFUNIT_UI_WINDOW_WIDTH for 430

{ Visibility=Private }
Class cDFUnitUIRunButton is a Button                
    Procedure OnClick
        Send ManualRunTests of ghoTestApplication
    End_Procedure
End_Class

{ Visibility=Private }
Class cDFUnitUIPanel is a Panel
    
    Procedure Construct_Object
        Forward Send Construct_Object
        
        { Visibility=Private }
        Property Handle poMainView
        
        Set Label to "DFUnit TestRunner"
        Set Size to DFUNIT_UI_WINDOW_HEIGHT DFUNIT_UI_WINDOW_WIDTH
        Set piMinSize to DFUNIT_UI_WINDOW_HEIGHT DFUNIT_UI_WINDOW_WIDTH
        Set piMaxSize to DFUNIT_UI_WINDOW_HEIGHT DFUNIT_UI_WINDOW_WIDTH
        Set Locate_Mode to Center_On_Screen
        
        { Visibility=Private }
        Object oClientArea is a ClientArea
            { Visibility=Private }
            Object oDFUnitTestRunner_vw is a View
                Set poMainView to oDFUnitTestRunner_vw
        
                Set Border_Style to Border_None
                Set Size to DFUNIT_UI_WINDOW_HEIGHT DFUNIT_UI_WINDOW_WIDTH
                Set piMinSize to DFUNIT_UI_WINDOW_HEIGHT DFUNIT_UI_WINDOW_WIDTH
                Set piMaxSize to DFUNIT_UI_WINDOW_HEIGHT DFUNIT_UI_WINDOW_WIDTH
                Set Location to 0 0
                Set Label to "DFUnit TestRunner"
                Set Maximize_Icon to False
                Set Minimize_Icon to False
                Set pbSizeToClientArea to False
                Set Sysmenu_Icon to False
                Set Caption_Bar to False
                Set View_Mode to Viewmode_Zoom
                
                { Visibility=Private }
                Object oOutputBox is a cDFUnitUIListReporter
                    Set Size to 187 397
                    Set Location to 6 10
                    Set peAnchors to anAll
                    Set piMinSize to 167 390
                End_Object
                Send AddReporter of ghoTestApplication oOutputBox
            
                { Visibility=Private }
                Object oRunTestsButton is a cDFUnitUIRunButton
                    Set Size to 30 397
                    Set piMinSize to 30 397
                    Set Location to 198 10
                    Set Label to "Run tests!"
                    Set peAnchors to anBottomLeftRight
                End_Object
            End_Object
        End_Object
    End_Procedure
    
    Procedure End_Construct_Object
        Forward Send End_Construct_Object
        Send Activate_View of (poMainView(Self))
        
        Boolean bAutoRunTests
        Delegate Get pbAutoRunTests to bAutoRunTests
        If (bAutoRunTests) Begin
            Send ManualRunTests of ghoTestApplication
        End
    End_Procedure
End_Class