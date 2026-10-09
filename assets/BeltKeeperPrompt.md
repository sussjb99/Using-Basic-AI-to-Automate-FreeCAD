** Macro Generation Prompt **
Task: Write a FreeCAD Python macro that creates a parametric 3D print model for a Belt Keeper using a Spreadsheet object for parameter management.
Requirements:
1.	Parametric Setup (FeaturePython):
o	Create a Spreadsheet::Sheet named "Spreadsheet" containing all driving parameters and derived expressions.
o	Build the geometry inside a Part::FeaturePython object ("BeltKeeper") with an execute(self, obj) method so that whenever values in the Spreadsheet are updated, the 3D model automatically recomputes and adjusts its dimensions.
o	Add a property link connecting BeltKeeper.Spreadsheet = sheet to enforce dynamic recomputation dependencies.
2.	Spreadsheet Parameters to Include:
o	User Inputs (Values & Aliases):
	BeltThickness: 3.5
	BeltWidth: 39.50
	Layers: 2
	WallThickness: 1.35
	KeeperDepth: 6.0
	FilletRadius: 1.0 (Outer vertical corner fillets)
	OuterFilletRadius: 0.3 (Entry rim roundovers)
	InnerFilletRadius: 0.5 (Inner cavity corner fillets)
	CutOvershoot: 0.5 (Extra length for clean Boolean subtraction)
o	Derived Formula Cell Aliases:
	InnerHeight: =BeltThickness * Layers
	InnerWidth: =BeltWidth
	OuterHeight: =InnerHeight + 2 * WallThickness
	OuterWidth: =InnerWidth + 2 * WallThickness
	OuterDepth: =KeeperDepth
3.	Geometry Construction Logic (execute method):
o	Cast all values fetched from sheet.get() to float() to avoid type/unit mismatch errors.
o	Outer Block: Centered at X=0, Y=0, starting at Z=0, sized to OuterWidth $\times$ OuterHeight $\times$ OuterDepth.
o	Inner Cutout: Centered at X=0, Y=0, extending from Z=-CutOvershoot to Z=OuterDepth + CutOvershoot, sized to InnerWidth $\times$ InnerHeight $\times$ (OuterDepth + 2 * CutOvershoot).
o	Fillets:
	Filter and apply FilletRadius to vertical outer corner edges.
	Filter and apply InnerFilletRadius to vertical inner cavity edges.
	Filter and apply OuterFilletRadius to front/back entry rim edges (at Z=0 and Z=OuterDepth).
4.	GUI/View Setup:
o	Set active view to axonometric/isometric and execute ViewFit after script completion.
