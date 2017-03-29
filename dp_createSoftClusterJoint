import pymel.core as pm
import maya.OpenMaya as om

#========================================================================================================================================================================================#
#= UI ===================================================================================================================================================================================#
#========================================================================================================================================================================================#

def dp_createSoftClusterJoint():
    """"""
    if pm.window ('CSCJ_window', ex=1): pm.deleteUI ('CSCJ_window')
    CSCJ_window = pm.window ('CSCJ_window', t='Soft Cluster & joint', w=200, h=100, s=0, rtf=1)
    CSCJ_form = pm.formLayout ('UI_mainForm', p=CSCJ_window)
    CSCJ_mainTabLayout = pm.tabLayout ('UI_mainTabs', childResizable=1, p=CSCJ_form)   
    #--- CREATE_TAB A -------------------------------------------------------------------------------------------------------------------------------------------------------------------#
    with pm.frameLayout ('Create', lv=0, bv=0, cll=0, mh=4, mw=4, p=CSCJ_mainTabLayout) as UI_clusterA: 
        with pm.formLayout ('UI_formGrpA', p=UI_clusterA) as UI_formGrpA:      
            #-- CREATE_NAME --#
            with pm.frameLayout ('UI_nameLayout', lv=False, bv=False, p=UI_formGrpA) as UI_nameLayout:
                with pm.rowLayout ('UI_nameGrp', numberOfColumns=2, adj=2, p=UI_nameLayout) as UI_nameGrp:
                    pm.text (l='Name : ', align='left', p='UI_nameGrp')
                    pm.textField ('UI_nameText', ed=1, text="", w=100, p=UI_nameGrp)     
            #-- CREATE_COMPONET --#
            with pm.frameLayout ('UI_componetLayout', l='Componet Selection', w=190, p=UI_formGrpA, bgc=[0.15, 0.15, 0.15], cll=1, cl=1, ec=lambda *args: resizeMainWindow(), cc=lambda *args: resizeMainWindow()) as UI_componetLayout:
                with pm.columnLayout ('UI_compTypeGrp', adj=1, rowSpacing=1, p=UI_componetLayout) as UI_compTypeGrp:
                    pm.checkBoxGrp ('UI_compTypeA', ncb=3, labelArray3=['Poly', 'Curve', 'All'], cw3=[70, 70, 70], va3=[1, 1, 1], cc=pm.Callback(UI_compSel), on3=pm.Callback(UI_compAll), of3=pm.Callback(UI_compAll), p=UI_compTypeGrp)
                    pm.checkBoxGrp ('UI_compTypeB', ncb=2, labelArray2=['Surface', 'Lattice'], cw2=[70, 70], va2=[1, 1], cc=pm.Callback(UI_compSel), p=UI_compTypeGrp)
                    pm.frameLayout ('UI_excludeObjects', l='exclude objects', w=190, p=UI_compTypeGrp, bgc=[0.07, 0.07, 0.07])
                    pm.textScrollList ('UI_textScrollList', allowMultiSelection=True)
                    pm.popupMenu()
                    pm.menuItem (l='Add', c=pm.Callback(UI_addExcludeObjects))
                    pm.menuItem (l='Delete', c=pm.Callback(UI_deleteExcludeObjects))
                    pm.menuItem (l='Clear', c=pm.Callback(UI_clearExcludeObjects))
            #-- CREATE_PIVOT --#    
            with pm.frameLayout ('UI_pivotLayout', l='Pivot', w=190, p=UI_formGrpA, bgc=[0.15, 0.15, 0.15], cll=1, cl=1, ec=lambda *args: resizeMainWindow(), cc=lambda *args: resizeMainWindow()) as UI_pivotLayout:
                with pm.rowLayout ('UI_selectGrp', numberOfColumns=3, adj=2, p=UI_pivotLayout) as UI_selectGrp:
                    pm.text (l='Center Pivot : ', align='left', p=UI_selectGrp)
                    pm.radioCollection ('UI_radioAllSelCP')
                    pm.radioButton ('UI_allCP', l='All CP', sl=True )
                    pm.radioButton ('UI_selectCP', l='Select CP' )
                with pm.rowLayout (adj=2, numberOfColumns=2, columnWidth2=(60, 10), p='UI_pivotLayout') as UI_pivot_textRow:
                    pm.text (l='Pick Pivot :', p= UI_pivot_textRow)
                    pm.textField ('UI_pivot', w=100, h=25, text='', p= UI_pivot_textRow)
                    pm.popupMenu (markingMenu=1)
                    pm.menuItem (l='piv Add',rp='N', c=pm.Callback(UI_pivAdd))
                    pm.menuItem (l='piv remove',rp='S', c=pm.Callback(UI_pivremove))
            #-- CREATE_FALLOFF --#
            with pm.frameLayout ('UI_falloffLayout', l='Falloff', w=190, p=UI_formGrpA, bgc=[0.15, 0.15, 0.15], cll=1, cl=1, ec=lambda *args: resizeMainWindow(), cc=lambda *args: resizeMainWindow()) as UI_falloffLayout:
                pm.columnLayout ('UI_falloffGrp', adj=1, p=UI_falloffLayout)
                with pm.rowLayout (numberOfColumns=3, columnWidth3=(63, 60, 1), p='UI_falloffGrp', adj=3) as UI_falloffButtonGrp:
                    pm.text (l='Soft Select:', p=UI_falloffButtonGrp)
                    pm.checkBox ('UI_soft_cb', l='', cc=pm.Callback(UI_changeSoftSelect), p=UI_falloffButtonGrp)
                    #pm.connectControl('UI_soft_cb', 'nurbsSphere1.tx')
                    pm.button (l='Reset', c=pm.Callback(UI_resetFalloff), w=40) 
                pm.separator (h=10, style='in', p='UI_falloffGrp')
                pm.optionMenuGrp ('UI_falloffModeOption', l='Falloff mode :', cc=pm.Callback(UI_changeFalloffMode), cl2=('left', 'left'), cw=(1, 80), adj=2, p='UI_falloffGrp') 
                pm.menuItem (l='Volume' )
                pm.menuItem (l='Surface')
                pm.menuItem (l='Global')
                pm.optionMenuGrp ('UI_falloffModeOption', e=1, sl=2)
                pm.optionMenuGrp ('UI_Interpolation', l='Interpolation :', cc=pm.Callback(UI_changeInterpolationValue), cl2=('left', 'left'), cw=(1, 80), adj=2, p='UI_falloffGrp')     
                pm.menuItem (l='None' )
                pm.menuItem (l='Linear' )
                pm.menuItem (l='Smooth' )
                pm.menuItem (l='Spline' )        
            #-- CREATE_BUTTON --#
            with pm.frameLayout ('UI_buttonLayout', lv=False, bv=False, p=UI_formGrpA) as UI_buttonLayout: 
                with pm.rowColumnLayout ('UI_createButtonGrp', nr=1, cs=[1,2], p=UI_buttonLayout, adj=1) as UI_createButtonGrp:
                    pm.button ('UI_CreateButton_cls', l='Cluster', w=105, h=35, c=pm.Callback(dp_createSoftCluster, 'softCluster'), p=UI_createButtonGrp, bgc=[1, 0.5, 0.5])    
                    pm.button ('UI_CreateButton_jnt', l='Joint', w=105, h=35, c=pm.Callback(dp_createSoftCluster, 'joint'), p=UI_createButtonGrp, bgc=[0.4, 0.4, 0.4])   
                pm.button ('UI_CloseButton', l='Close', w=50, h=35, c='import pymel.core as pm\npm.deleteUI (\'CSCJ_window\')', p=UI_buttonLayout, bgc=[0.15, 0.15, 0.15])     
    #--- CREATE_TAB B ------------------------------------------------------------------------------------------------------------------------------------------------------------------#
    with pm.frameLayout ('Utility', lv=0, bv=0, cll=0, mh=4, mw=4, p=CSCJ_mainTabLayout) as UI_clusterB: 
        with pm.formLayout ('UI_formGrpB', p=UI_clusterB) as UI_formGrpB:
            with pm.columnLayout ('UI_utilityButtonGrp', adj=1, rowSpacing=1, p=UI_formGrpB) as UI_utillityButtonGrp:
                pm.button (l='C.J Move Pivot', w=210, h=25, c=pm.Callback(dp_moveClsPivot), p=UI_utillityButtonGrp, bgc=[0.15, 0.15, 0.15])          
                pm.button (l='C.J Detach Geometry', w=210, h=25, c=pm.Callback(dp_detachGeo), p=UI_utillityButtonGrp, bgc=[0.15, 0.15, 0.15])        
            with pm.rowColumnLayout ('UI_LocButtonGrp', nr=1, cs=[1,2], p=UI_formGrpB, adj=1) as UI_LocButtonGrpB:
                pm.button (l='Loc Me', w=105, h=25, c=pm.Callback(dp_createLocator, 'Me'), p=UI_LocButtonGrpB, bgc=[1, 0.8, 0.8])  
                pm.button (l='Loc Center', w=105, h=25, c=pm.Callback(dp_createLocator, 'Center'), p=UI_LocButtonGrpB, bgc=[0.8, 1, 0.8])     
            UI_CloseButtonB = pm.button ('UI_CloseButton', l='Close', w=50, h=35, c='import pymel.core as pm\npm.deleteUI (\'CSCJ_window\')', p=UI_formGrpB, bgc=[0.15, 0.15, 0.15])      
    #--- form set A --------------------------------------------------------------------------------------------------------------------------------------------------------------------#              
    pm.formLayout(UI_formGrpA, 
                  edit=True,
                  af=[(UI_nameLayout, 'left', 0),
                      (UI_nameLayout, 'right', 0),
                      (UI_nameLayout, 'top',0),
                      (UI_componetLayout, 'left', 0),
                      (UI_componetLayout, 'right', 0),
                      (UI_componetLayout, 'top', 0),
                      (UI_pivotLayout, 'left', 0),
                      (UI_pivotLayout, 'right', 0),
                      (UI_pivotLayout, 'top', 0),
                      (UI_falloffLayout, 'right', 0),
                      (UI_falloffLayout, 'left', 0),
                      (UI_falloffLayout, 'top', 0),
                      (UI_falloffLayout, 'bottom', 0),
                      (UI_buttonLayout, 'right', 0),
                      (UI_buttonLayout, 'left', 0),
                      (UI_buttonLayout, 'bottom', 0)],
                  ac=[(UI_componetLayout, 'top', 2, UI_nameLayout),
                      (UI_pivotLayout, 'top', 2, UI_componetLayout),
                      (UI_falloffLayout, 'top', 2, UI_pivotLayout),
                      (UI_falloffLayout, 'bottom', 2, UI_buttonLayout)])  
    #--- form set B --------------------------------------------------------------------------------------------------------------------------------------------------------------------# 
    pm.formLayout(UI_formGrpB, 
                  edit=True,
                  af=[(UI_LocButtonGrpB, 'left', 0),
                      (UI_LocButtonGrpB, 'right', 0),
                      (UI_LocButtonGrpB, 'top',0), 
                      (UI_utillityButtonGrp, 'right', 0),
                      (UI_utillityButtonGrp, 'left', 0),
                      (UI_utillityButtonGrp, 'top', 0),
                      (UI_CloseButtonB, 'right', 0),
                      (UI_CloseButtonB, 'left', 0),
                      (UI_CloseButtonB, 'bottom', 0)],
                  ac=[(UI_LocButtonGrpB, 'top', 2, UI_utillityButtonGrp),
                      (UI_LocButtonGrpB, 'bottom', 2, UI_CloseButtonB)])                                   
    #------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------#
    pm.formLayout(CSCJ_form,
              edit=True,
              af=[(CSCJ_mainTabLayout, 'top', 0),
                  (CSCJ_mainTabLayout, 'left', 0),
                  (CSCJ_mainTabLayout, 'bottom', 0),
                  (CSCJ_mainTabLayout, 'right', 0)])
    
    pm.showWindow(CSCJ_window)
    resizeMainWindow()
    
#========================================================================================================================================================================================#
#= UI_def ===============================================================================================================================================================================#
#========================================================================================================================================================================================#
def resizeMainWindow():
    totalHeight = 0
    for child in pm.formLayout ('UI_formGrpA', q=1, ca=1):
        h = pm.frameLayout (child, q=1, h=1)
        if pm.frameLayout (child , q=1, collapse=1): h = 20
        totalHeight += h
    pm.window ('CSCJ_window', e=1, h=totalHeight)
#----------------------------------------------------------------------    
def UI_compSel ():
    bt_poly = pm.checkBoxGrp ('UI_compTypeA',q=1, v1=1)
    bt_curve = pm.checkBoxGrp ('UI_compTypeA',q=1, v2=1)
    bt_surface = pm.checkBoxGrp ('UI_compTypeB', q=1, v1=1)
    bt_lattice = pm.checkBoxGrp ('UI_compTypeB', q=1, v2=1) 
        
    if  bt_poly + bt_curve + bt_surface +bt_lattice < 4:
        pm.checkBoxGrp ('UI_compTypeA', e=1, v3=0)
    if  bt_poly + bt_curve + bt_surface +bt_lattice == 4:
        pm.checkBoxGrp ('UI_compTypeA', e=1, v3=1)
#----------------------------------------------------------------------
def UI_compAll ():
    bt_all = pm.checkBoxGrp ('UI_compTypeA',q=1, v3=1)
    if bt_all == 0:
        pm.checkBoxGrp ('UI_compTypeA', e=1, v1=0, v2=0)
        pm.checkBoxGrp ('UI_compTypeB', e=1, v1=0, v2=0)
    if bt_all == 1:
        pm.checkBoxGrp ('UI_compTypeA', e=1, v1=1, v2=1)
        pm.checkBoxGrp ('UI_compTypeB', e=1, v1=1, v2=1)
#----------------------------------------------------------------------
def UI_addExcludeObjects():
    sel = pm.ls (sl=1, ap=1)
    excludeItems = pm.textScrollList ('UI_textScrollList', q=1, ai=1)

    for obj in sel:
        if not excludeItems:
            pm.textScrollList ('UI_textScrollList', e=1, a=obj)
        elif not obj in excludeItems:
            pm.textScrollList ('UI_textScrollList', e=1, a=obj)
#----------------------------------------------------------------------
def UI_clearExcludeObjects():
    pm.textScrollList ('UI_textScrollList', e=1, ra=1)
#----------------------------------------------------------------------
def UI_deleteExcludeObjects():
    selItems = pm.textScrollList ('UI_textScrollList', q=1, si=1)
    pm.textScrollList ('UI_textScrollList', e=1, ri=selItems)
#----------------------------------------------------------------------              
def UI_pivAdd():
    select_piv = pm.ls (sl=1)
    if not select_piv:
        print 'Please select an object.'
    else:  
        if select_piv[0].type() == 'transform' or select_piv[0].type() == 'joint':
            pm.textField ('UI_pivot', e=1, text=select_piv[0])
#----------------------------------------------------------------------
def UI_pivremove():
    pm.textField ('UI_pivot', e=1, text='')
#----------------------------------------------------------------------    
def UI_changeSoftSelect():
    selIndex = pm.checkBox ('UI_soft_cb', q=1, v=1)
    pm.softSelect (sse=selIndex)
#----------------------------------------------------------------------
def UI_changeFalloffMode():
    selIndex = pm.optionMenuGrp ('UI_falloffModeOption', q=1, sl=1) - 1
    pm.softSelect (ssf=selIndex)
#----------------------------------------------------------------------   
def UI_changeInterpolationValue():
    interp = pm.optionMenuGrp ('UI_Interpolation', q=1, sl=1)
    if interp == 1 :   pm.softSelect (ssc='1,0,2,1,1,2')    
    elif interp == 2 : pm.softSelect (ssc='1,0,1,0,1,1')    
    elif interp == 3 : pm.softSelect (ssc='1,0,2,0,1,2')    
    elif interp == 4 : pm.softSelect (ssc='1,0,3,0,1,3')
#----------------------------------------------------------------------
def UI_resetFalloff():
    pm.optionMenuGrp ('UI_Interpolation', e=1, sl=3)
    pm.optionMenuGrp ('UI_falloffModeOption', e=1, sl=1)
    pm.softSelect (ssf=0)
    pm.softSelect (e=1, ssc='0,1,2,1,0,2')
#----------------------------------------------------------------------
#========================================================================================================================================================================================#
#= dp_def ===============================================================================================================================================================================#
#========================================================================================================================================================================================#

def dp_softSelection(selType, excludeObj):
    sel = om.MSelectionList()
    softSel = om.MRichSelection()
    om.MGlobal.getRichSelection(softSel)
    softSel.getSelection(sel)
    dagPath = om.MDagPath()
    component = om.MObject()
    iter = om.MItSelectionList(sel)
    obj, elements, weights, transformObj = [], [], [], []

    while not iter.isDone():
        iter.getDagPath(dagPath, component) 
        
        if dagPath.hasFn(om.MFn.kTransform) == 1:
            node = pm.PyNode(dagPath)
            node = dagPath.fullPathName()   
            transformObj.append(node)
            iter.next()
            continue
        else:
            dagPath.pop()
            node = dagPath.fullPathName()   
            getShape = pm.ls(node)[0].getShape()
        type = pm.nodeType(getShape)
        
        if not selType.count(type) or excludeObj.count(node[1:]):
            iter.next()
            continue
        obj.append(node)
 
        if type == 'nurbsCurve' or type == 'mesh':
            fnComp = om.MFnSingleIndexedComponent(component)  
        if type == 'nurbsSurface':
            fnComp = om.MFnDoubleIndexedComponent(component)
        if type == 'lattice':
            fnComp = om.MFnTripleIndexedComponent(component)
     
        getWeight = lambda i: fnComp.weight(i).influence() if fnComp.hasWeights() else 1.0  
      
        for i in range(fnComp.elementCount()):
            if type == 'nurbsSurface':  
                indexListU = om.MIntArray()
                indexListV = om.MIntArray()
                fnComp = om.MFnDoubleIndexedComponent(component)
                fnComp.getElements(indexListU, indexListV)
                elements.append('%s.cv[%i][%i]' % (node, indexListU[i] ,indexListV[i]))   
            elif type == 'lattice':
                indexListS = om.MIntArray()
                indexListT = om.MIntArray()
                indexListU = om.MIntArray()
                fnComp = om.MFnTripleIndexedComponent(component)
                fnComp.getElements(indexListS, indexListT, indexListU)
                elements.append('%s.pt[%i][%i][%i]' % (node, indexListS[i], indexListT[i], indexListU[i]))       
            else:
                if type == 'nurbsCurve':
                    elements.append('%s.cv[%i]' % (node, fnComp.element(i)))    
                if type == 'mesh':
                    elements.append('%s.vtx[%i]' % (node, fnComp.element(i)))
            weights.append(getWeight(i)) 
        iter.next()
    
    return obj, elements, weights, transformObj
#----------------------------------------------------------------------
def dp_centerPivot(objSel):         
    posX, posY, posZ = [], [], []
    for i in objSel:      
        if pm.nodeType (objSel) == 'transform': 
            pos = pm.xform (i, q=1, rp=1, ws=1)
        else:                                  
            pos = pm.xform (i, q=1, t=1, ws=1)
        posX.append(pos[0])
        posY.append(pos[1])
        posZ.append(pos[2])
    centerPivot = (float(sum(posX)/len(objSel)), float(sum(posY)/len(objSel)), float(sum(posZ)/len(objSel)))
    return centerPivot
#----------------------------------------------------------------------
def dp_createLocator(mode):
    objSel = pm.ls (sl=1, fl=1)
    locList = []
    
    if not objSel:
        pm.spaceLocator (n='loc', p=[0, 0, 0])
    else:
        if mode == 'Me':
            for objSel in objSel:                     
                locNaming = 'Loc_' + objSel
                locSel = [pm.spaceLocator (n=locNaming)]
                locList += locSel
                if pm.nodeType (objSel) == 'transform':
                    objectTrans = pm.xform (objSel, q=1, rp=1, ws=1)
                    objOrder = pm.getAttr ('%s.rotateOrder' %objSel)        
                else:
                    objectTrans = pm.xform (objSel, q=1, t=1, ws=1)
                objectRotate = pm.xform (objSel, q=1, ro=1, ws=1)           
                pm.setAttr ('%s.t' %locSel[0], objectTrans[0], objectTrans[1], objectTrans[2])
                pm.setAttr ('%s.r' %locSel[0], objectRotate[0], objectRotate[1], objectRotate[2])
                if pm.nodeType (objSel) == 'transform':
                    pm.setAttr ('%s.rotateOrder' %locSel[0], objOrder) 
                    pm.xform (locSel[0], p=1, roo='xyz') 
        elif mode == 'Center':
            centerPos = dp_centerPivot(objSel)
            locNaming = 'Loc_' + objSel[0] + '_C' 
            locList = pm.spaceLocator (n=locNaming)
            pm.setAttr (locList + '.t', centerPos[0], centerPos[1], centerPos[2])
        pm.select (locList, r=1)
#----------------------------------------------------------------------        
#========================================================================================================================================================================================#
#= create_def ===========================================================================================================================================================================#
#========================================================================================================================================================================================#

#cluster#
def dp_createSoftCluster(createMode):
    ''''''
    selPoint = pm.ls(sl=1, fl=1)
    if selPoint == []:
        print 'There is no selectable object.'
        return
    name = pm.textField ('UI_nameText', q=1, text=1)
    
    if name == '':      name = createMode
    pivSel = pm.textField ('UI_pivot', q=1, text=1)  
     
    if pivSel == '' or pm.ls(pivSel) == []:    mode = pm.radioCollection ('UI_radioAllSelCP',q=1, sl=1)
    else:                                      mode = 'pickPivot'
    
    selType = []
    if pm.checkBoxGrp ('UI_compTypeA',q=1, v1=1)  == 1:  selType += ['mesh']
    if pm.checkBoxGrp ('UI_compTypeA',q=1, v2=1) == 1:   selType += ['nurbsCurve']
    if pm.checkBoxGrp ('UI_compTypeB', q=1, v1=1) == 1:  selType += ['nurbsSurface']
    if pm.checkBoxGrp ('UI_compTypeB', q=1, v2=1) == 1:  selType += ['lattice']
    
    excludeObj = pm.textScrollList ('UI_textScrollList', q=1, ai=1)
    
    if createMode == 'softCluster':
        if pm.softSelect (q=1, sse=1) == 0 or pm.softSelect (q=1, ssd=1) <= 0:
            cls = pm.cluster (n=name, relative=True)
            pm.select(selPoint)
            pm.rename (cls[1], 'Cls_'+name)
            if mode == 'pickPivot':
                centerPivot = dp_centerPivot(pm.ls(pivSel))
                cls[1].scalePivot.set(centerPivot)
                cls[1].rotatePivot.set(centerPivot)
                cls[1].getShape().origin.set(centerPivot)
        else:
            softSelectionData = dp_softSelection(selType, excludeObj)
            getObj, getSoftElementData, getSoftWeights, transformObj = softSelectionData

            if mode == 'UI_selectCP':
                centerPivot = dp_centerPivot(selPoint)
            elif mode == 'UI_allCP':  
                centerPivot = dp_centerPivot(getSoftElementData+transformObj)
            elif mode == 'pickPivot':
                centerPivot = dp_centerPivot(pm.ls(pivSel))   
            pm.select (cl=1)
            
            cls = pm.cluster (n=name, relative=True)
            pm.rename(cls[1], 'Cls_'+name)
            cls[1].scalePivot.set(centerPivot)
            cls[1].rotatePivot.set(centerPivot)
            cls[1].getShape().origin.set(centerPivot)
            cls_set = pm.listConnections (str(cls[0]), type = 'objectSet')

            for obj in getObj:
                pm.sets ((cls_set[0]), fe = obj)
                pm.percent(cls[0], obj, v=0)              
 
            for i in range(len(getSoftElementData)):
                pm.percent(cls[0], getSoftElementData[i], v=getSoftWeights[i])
            if transformObj != []:
                for obj in transformObj:
                    pm.sets ((cls_set[0]), fe = obj)   
        pm.select (cls[1], r=True)

    elif createMode == 'joint':
        print 'a'
        if pm.softSelect (q=1, sse=1) == 0 or pm.softSelect (q=1, ssd=1) <= 0:
            jnt = pm.joint (n=name, r=1)
            pm.select(selPoint)
            pm.rename (jnt[1], 'Jnt_' + name)
            #if mode == 'pickPivot':
            #    centerPivot = dp_centerPivot(pm.ls(pivSel))
            #    cls[1].scalePivot.set(centerPivot)
            #    cls[1].rotatePivot.set(centerPivot)
            #    cls[1].getShape().origin.set(centerPivot)
        #softSelectionData = dp_softSelection(selType, excludeObj)
       
        #getObj = softSelectionData[0]
        #getSoftElementData = softSelectionData[1]
        #getSoftWeights = softSelectionData[2]
        
#----------------------------------------------------------------------   
#========================================================================================================================================================================================#
#= Utility_def ==========================================================================================================================================================================#
#========================================================================================================================================================================================#
#----------------------------------------------------------------------   
def dp_moveClsPivot():
    selObj = pm.ls(sl=1)
    if len(selObj) != 2:
        print 'Please select a cluster (or joint) first, point to the location you want to move!'
        return
    moveObj = selObj.pop(0)
    targetPivot = dp_centerPivot(selObj)
    if moveObj.type() == 'joint':
        skins = list(set(pm.listConnections(moveObj, type='skinCluster')))
        for skin in skins:
            pm.skinCluster(skin, e=1, mjm=1)
        parentJnt = moveObj.getChildren()
        if parentJnt != []:
            for pJnt in parentJnt:
                pm.parent(pJnt, w=1)               
        pm.move(targetPivot[0], targetPivot[1], targetPivot[2], moveObj)
        if parentJnt != []:
            for pJnt in parentJnt:
                pm.parent(pJnt, moveObj)   
        for skin in skins:
            pm.skinCluster(skin, e=1, mjm=0)  
        pm.select(moveObj,selObj)    
    elif moveObj.getShape().type() == 'clusterHandle':
        moveObj.origin.set(targetPivot)
        moveObj.rotatePivot.set(targetPivot)
        moveObj.scalePivot.set(targetPivot)
    else:
        print 'Please select a cluster (or joint) first, point to the location you want to move!'
#----------------------------------------------------------------------   
#joint#
def dp_createSoftjoint(): 
    softElementData = dp_softSelection()
    selection = ["%s.vtx[%d]" % (el[0], el[1])for el in softElementData ] 
    pm.select(selection, r=True)
    jointadd = pm.joint(relative=True)
 
    pm.skinCluster ('skinCluster1', e=1, ug=1, dr=4, ps=0, ns=10, lw=1, wt=0, ai= jointadd )
    for i in range(len(softElementData)):
        pm.skinPercent ('skinCluster1', selection[i], transformValue=(jointadd,softElementData[i][2]))
#----------------------------------------------------------------------
def dp_detachGeo():
    getObj = pm.ls(sl=1)
    if len(getObj) < 2:
        return
    firstObj = getObj.pop(0)
    # joint 
    if firstObj.type() == 'joint':
        for geo in getObj:
            if geo.type() == 'transform':
                geo = pm.listRelatives(geo, s=True, ni=True, pa=True)[0]
                skin = pm.ls (pm.listHistory(geo, pdo=1, gl=1), type='skinCluster')
                if pm.skinCluster (skin, q=1, inf=1).count(firstObj) == 1:
                    pm.skinCluster (skin, e=1, ri=firstObj)
                if pm.skinCluster (skin, q=1, inf=1) == []:
                    pm.delete (skin)
    # cluster
    elif firstObj.getShape().type() == 'clusterHandle':
        types = ['mesh', 'nurbsCurve', 'nurbsSurface', 'lattice']
        for geo in getObj:
            if geo.type() != 'joint':
                geoType = geo.getShape().type()
                if types.count(geoType) == 1: 
                    pm.cluster (firstObj, e=1, g=geo, rm=1, rg=1)
    else:
        print 'Please cluster (or joint) first, then geometries you want to detach!'

#start#
dp_createSoftClusterJoint()
