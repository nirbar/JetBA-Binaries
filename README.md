JetBA WiX extension features:
- JetBA: The most comprehensive, fully customizable, extensible, WPF-based BootstrapperApplication.
- JetBA++: The only native fully customizable, extensible, Qt-based BootstrapperApplication.
- Preprocessor extension:
  - Harvest directly from WiX source code by executing Heat commands. The command is passes to heat.exe with the following changes:
    - Added support to use DuplicateFile where possible:
      - -cp NameSizeHash: Files with the same name, size, and MD5 hash are duplicated
      - -cp NameVersion: File with the same name and version are duplicated. Files without a version are handled on NameSizeHash policy
    - The "-out" parameter can be omitted
    - "SourceDir" is replaced with preprocessed folder path in File/@Source and Payload/@SourceFile attributes
    ~~~~~~~
    <?pragma heat.dir "$(sys.SOURCEFILEDIR)..\bin\Release" -cg BIN -dr INSTALLFOLDER -ag?>
    ~~~~~~~
  - Collection variables
    ~~~~~~~
    <?pragma tuple.NIR a; b; c?>
    <?pragma tuple.BAR x; y; z?>
    <?foreach i in $(tuple_range.BAR())?>
    <Component Feature="ProductFeature" Directory="INSTALLFOLDER">
      <File Source="$(sys.SOURCEFILEPATH)" Name="Product.$(tuple.NIR($(var.i))).$(tuple.BAR($(var.i))).wxs"/>
    </Component>
    <?endforeach?>
    <?pragma endtuple.BAR?>
    <?pragma endtuple.NIR?>
    ~~~~~~~
  - Generate random Id. 
    Useful when deploying files with same name to different target folders:
    ~~~~~~~
  	<ComponentGroup Id="random">
  		<Component Directory="Product.Dir">
  		<File Source="$(sys.SOURCEFILEPATH)" Id="$(jet.RandomId())"/>
  		</Component>
  		<Component Directory="INSTALL_FOLDER">
  		<File Source="$(sys.SOURCEFILEPATH)" Id="$(jet.RandomId())"/>
  		</Component>
  	</ComponentGroup>
    ~~~~~~~
  - Get a running index. 
    On each call get the next index, starting at 1.
    ~~~~~~~
  	<ComponentGroup Id="random">
  		<Component Directory="Product.Dir">
  		<File Source="$(sys.SOURCEFILEPATH)" />
            <util:XmlFile ... Sequence="$(jet.Index())"/>
            <util:XmlFile ... Sequence="$(jet.Index())"/>
  		</Component>
  	</ComponentGroup>
    ~~~~~~~
  - Generate random or pseudo-random Guid. 
    ~~~~~~~
  	<ComponentGroup Id="random">
      <?foreach account in localService;localSystem;networkService;applicationPoolIdentity?>
      <Component Id="webapp.$(var.account)" Guid="$(jet.Guid($(var.account)))" Transitive="yes">
        <Condition><![CDATA[IIS_ACCOUNT ~= "$(var.account)"]]></Condition>
        <CreateFolder />
        <iis:WebAppPool Id="$(var.account)" Name="WebUI" Identity="$(var.account)" ManagedRuntimeVersion="v4.0" ManagedPipelineMode="Integrated" />
        <iis:WebVirtualDir Id="$(var.account)" Directory="INSTALLFOLDER" Alias="WebUI" WebSite="WebUI">
          <iis:WebApplication Id="$(var.account)" Name="WebUI" WebAppPool="$(var.account)" />
        </iis:WebVirtualDir>
      </Component>
      <?endforeach?>
  	</ComponentGroup>
    ~~~~~~~
- Detect bundles and get their versions in a variable:
    ~~~~~~~
    <jet:BundleSearch UpgradeCode="{1D1DB5E6-E0D8-3103-8570-369A82A9BF33}" VersionVariable="DetectedVC2013x86Version" NamePattern="\bx86\b"/>
    ~~~~~~~
- RebootBoundary attrbiute on bundle packages: force reboot after the package if any preceding package required reboot
