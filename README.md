# appmark
Mobile Android IDE

This is android app where you can build your own apps using Java

support: https://unikalni4elovek.000webhostapp.com/donate

# struct (IN DEVELOPING):
SOURCE DIRECTORY: appmark/app/src/main/java/com/vipapp/appmark2 <br>

    -activities  # activities storage 
      
      /**
      *    ACTIVITY ARCHITECTURE:
      *  onCreateView() -> int - return argument that will be passed in setContentView()
      *  createView() - additional method to set view
      *  findViews() - findViewById(...) calls
      *  setCallbacks() - set all callbacks
      *  init() - set default values for variables
      *  setup() - set default state for views
      **/
      
      BaseActivity:  # main activity class 

        > callbacks: ArrayList<PushCallback<ActivityResult>>  # list with all onActivityResult callbacks
        > onLoadCallbacks: ArrayList<PushCallback<OnLoadItem>>  # list with all onItemLoad callback 
        
        *exit_with_permission_error() -> exit from app and show permission error
        *onLoad(item: OnLoadItem) -> call all 'onLoadCallback' and onLoadCallback(...)
        
        *addOnActivityResultCallback(callback: ...) -> add callback to 'callbacks'
        *addOnLoadCallback(callback: ...) -> add callback to 'onLoadCallback'
        
        *f(id: int) -> findViewById(id)
        
        - Overridable -
        *onLoadCallback(item: OnLoadItem) in onLoad(...)
        *onCreateView() in onCreate(...)
        *createView() in onCreate(...)
        *findViews() in onCreate(...)
        *setCallbacks() in onCreate(...)
        *init() in onCreate(...)
        *setup() in onCreate(...)
        
        - Overriden -
        *onCreate(...) -> set default orientation and call architecture methods
        *onRequestPermissionResult(...) -> exit from app if any permission rejected or recreate activity
        *onActivityResult(...) -> call all 'callbacks'
        *findViewById(...) -> marked as deprecated to do not forgot use f()

      IntroActivity(BaseActivity) -> activity with introduce animation and permission requesting
      CodeActivity(BaseActivity) -> activity with code editor and file manager
      MainActivity(BaseActivity) -> activity with project manager
      DebugActivity(BaseActivity) -> activity with error view

      StringEditorActivity(BaseActivity)
      SettingsActivity(BaseActivity)
      ViewDesignActivity(BaseActivity)

    -adapter  # adapters storage

      # it's recommended to use DefaultMenu class to work with recycler
      DefaultAdapter:  # main adapter class that uses DefaultMenu class as support for build

        > menu: DefaultMenu  # support class
        > viewHolderName: String  # view holder class name
        > list: ArrayList  # list with elements
        > xml_source: int  # layout resource
        > recyclerView: RecyclerView  # parent recycler
        > callbacks: ArrayList<PushCallback<Item>>  # item push callbacks

        *pushArray(...) -> support methods to push array from menu to recycler
        *transferObjectToMenu(item: Item) -> support method to push item from recycler to menu
        *onPush(item: Item) -> support method to push item from recycler to menu (execs all 'callbacks')

        *addOnPushCallback(callback: PushCallback<Item>) -> add 'callback' to 'callbacks'

        *onRecyclerPushed(recycler: RecyclerView) -> connect recycler with adapter

        *update() -> update recycler view list by getting new one from menu

        - Overriden -
        *onCreateViewHolder(...) -> set view holder from 'viewHolderName'
        *onBindViewHolder(...) -> call menu.bind(...)
        *getItemViewType(...) -> call menu.getItemViewType(...)
        *getItemCount(...) -> call list.size()
        
    -alert  # alert dialogs storage
      
      -confirm  # confirm alerts dialog storage
      
        DeleteFileDialog(ConfirmDialog) -> dialog with file delete confirmation
        DeleteProject(ConfirmDialog) -> dialog with project delete confirmation
      
      -strings_list_editor  # strings list editor dialogs storage
      
        ProjectSettingsDialog -> dialog with project settings editor
        
      ConfirmDialog:  # dialog with agree and decline button
      
        # show dialog with title 'title_text', button-agree with text 'text_confirm',
        # button-decline with text 'text_cancel' that pushes result into 'result'.onComplete()
        *show(title_text: String, text_confirm: String, text_cancel: String, result: PushCallback<Boolean>) 
        
      LoadableInfoDialog(AlertDialog):  # dialog with loadable information 
        # create dialog with title as 'title' that loading until callback that is pushing in
        # 'callback' do not receive loaded string
        *LoadableInfoDialog(title: String or string-res, callback: PushCallback<PushCallback<String>>)
        
      InfoDialog(LoadableInfoDialog):  # dialog that shows information
        *InfoDialog(title: String, message: String) -> immidiately push 'message' in LoadableInfoDialog
        
      LoadingDialog:  # loading dialog
        *show(...) -> hide previous dialog and show new one on current activity with '...' args
        *hide() -> hide previous dialog
        *setTitle(title: String or string-res) -> set dialog title as 'title'
        
      (1): ArrayList<TransformedItem<String, String>>>
      StringsListEditor:  # dialog with strings list editor
        # show list editor with default values
        *show(title: String, strings: (1), callback: PushCallback<(1)>)
        
      CreateProjectDialog -> dialog with project creation
      EditProject -> dialog with project edition
      
    -callbacks  # callbacks storage
    
      ActivityLifecycleCallback(ActivityLifecycleCallbacks) -> actiity lifecycle callback with updating context in ContextUtils
      
      Mapper<FirstType, SecondType>:  # SecondType from FirstType converter
        *abstract map(item: FirstType) -> SecondType
        
      Predicate<Type>:  # check object of type Type
        *abstract test(object: Type) -> boolean
      
      PushCallback<Type>:  # callback to push object of type Type
        *onComplete(object: Type)

    -compiler  # package with compiler wrapper
    
      Compiler:  # com.vipapp.fjc.ApkBuilder wrapper
        *init() -> initialize Aapt and androidJar with ApkBuilder
        *needInit() -> boolean; check if androidJar already exists
        
        # call compileRelease('project', 'null', 'callback') and
        # compile without certificate (now AppMark uses only it)
        *compileDebug(project: Project, callback: ApkBuilderCallBack)
        
        # compile apk with ApkBuilder
        *compileRelease(project: Project, certificate: ApkBuilderCert, callback: ApkBuilderCallBack)
        
    -exception  # package with AppMark exceptions
    
      IncorrectAIFName(Exception):  # throws when .aif file name is incorrect
        IncorrectAIFName(name: String) -> name setup]
        
    -holder  # directory with view holders
    
      ChooserHolder(ViewHolder) -> view holder in dialog chooser
      EmptyHolder(ViewHolder) -> empty view holder (using for create holders in runtime)
      FileHolder(ViewHolder) -> view holder in file manager
      ImageHolder(ViewHolder) -> view holder in gallery
      InsertSymbolHolder(ViewHolder) -> view holder in "fast symbols" in CodeActivity
      MainMenuHolder(ViewHolder) -> view holder in menu in MainActivity
      ProjectHolder(ViewHolder) -> view holder in projects list in MainActivity
      StringsHolder(ViewHolder) -> view holder in strings list in StringEditorActivity
      StringListEditorHolder(ViewHolder) -> view holder in StringsListEditorDialog