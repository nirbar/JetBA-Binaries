# v5.2.0

Support embedding and extracting parameters in signed MSI and binaries

# v5.0.0

- heat.dir \[-cp None|NameVersion|NameSizeHash\]
  - _NameSizeHash_: If files have same name, size, and MD5 hash then use CopyFile to reduce package size
  - _NameVersion_: If files have same name andversion then use CopyFile to reduce package size. For files without a version, fallback to _NameSizeHash_
  - _None_ (default): Not attemtpting to reduce package size

# v4.7.1

- Sign JetBA assemblies with strong name

# v4.7.0

- Fix ValidateAction to persist exception caption and level when catching an InputValidationException

# v4.6.2

- When zipping log files, skip adding same file if unmodified.

# v4.6.1

- Support adding files to an existing log, to allow post-reboot log files additions

# v4.6.0

- _JetBootstrapperApplication.HasJetBaExecuted_: Helper property to tell after reboot if WPF UI has been presented yet or not
- Only set burn variables on actual value changes, to reduce redundant log messages. PropertyChanged notification are unchanged to allow intentional event fire.

# v4.5.2

- Fix zipping log files when one of the packages is a Windows Update (*.msu) which have no LogPathVariable

# v4.5.1

- Fix stack overflow on AggregateException in InputValidationsViewModel
- _ApplyViewModel.ErrorMessage_: Not setting the message if the package isn't vital.

# v4.5.0

- _InputValidationsViewModel.ValidateAction()_: Helper method to execute an action and add result with specified caption in text

# v4.4.0

- _ViewModelBase.StartLengthyTask()_: Helper method and status property to launch time consuming tasks
- _InputValidationsViewModel.InitializePage()_: Helper method to initialize variables per page

# v4.3.1

- Exit with _ERROR_SUCCESS_REBOOT_INITIATED_ (1641) when reboot is initiated.
- Support ex/cluding build-in variables on MSBuild parameter.
- Builtin parameters in separate list

# v4.3.0

- _VariablesViewModel.EscapeAsCommandLineArg()_: Helper method to escape variables for command line of _ExePackage_
