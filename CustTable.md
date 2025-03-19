# 📘 CustTable Metadata Knowledge Base

## 📌 Full XML Content

- **Name:** `CustTable`
- **SourceCode:**
  - **Declaration:** `public class CustTable extends common
{
}`
  - **Methods:**
    - **Method:**
      - **Name:** `address`
      - **Source:** `display LogisticsAddressing address()
    {
        return this.postalAddress().Address;
    }`
    - **Method:**
      - **Name:** `addressComplement_BR`
      - **Source:** `/// <summary>
    ///    Gets the building name of the customer.
    /// </summary>
    /// <returns>
    ///    Customer BuildingCompliment.
    /// </returns>
    display LogisticsAddressBuildingCompliment addressComplement_BR()
    {
        return this.postalAddress().BuildingCompliment;
    }`
    - **Method:**
      - **Name:** `amountChargedNotPosted`
      - **Source:** `/// <summary>
    /// Gets the sum of the <c>amountToAccount</c> field for the current customer account.
    /// </summary>
    /// <returns>
    /// The total sum of the customer account.
    /// </returns>
    display AmountMST amountChargedNotPosted()
    {
        RetailTransactionTable transactionTable;

        select sum(AmountToAccount) from transactionTable
            where transactionTable.EntryStatus == RetailEntryStatus::None
               && transactionTable.CustAccount == this.AccountNum;

        return transactionTable.AmountToAccount;
    }`
    - **Method:**
      - **Name:** `balanceAllCurrency`
      - **Source:** `display AmountCur balanceAllCurrency(CustCurrencyCode _currencyCode = this.Currency)
    {
        return this.CustVendTable::balanceAllCurrency(_currencyCode);
    }`
    - **Method:**
      - **Name:** `balanceCurPerDate`
      - **Source:** `AmountCur balanceCurPerDate(TransDate    _transactionDate    = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()),
                                       CurrencyCode _currencyCode       = CompanyInfoHelper::standardCurrency())
    {
        return this.CustVendTable::balanceCurrency(dateNull(), _transactionDate, _currencyCode, _currencyCode);
    }`
    - **Method:**
      - **Name:** `balanceCurrency`
      - **Source:** `AmountCur balanceCurrency(TransDate     _fromDate       = dateNull(),
                              TransDate     _toDate         = dateMax(),
                              CurrencyCode  _fromCurrency   = '',
                              CurrencyCode  _toCurrency     = '')
    {
        return this.CustVendTable::balanceCurrency(_fromDate,
                                                   _toDate,
                                                   _fromCurrency,
                                                   _toCurrency);
    }`
    - **Method:**
      - **Name:** `balanceMST`
      - **Source:** `display AmountMST balanceMST(FromDate   _fromDate   = dateNull(),
                                 ToDate     _toDate     = dateMax())
    {
        return this.CustVendTable::balanceMST(_fromDate, _toDate);
    }`
    - **Method:**
      - **Name:** `balancePerDate`
      - **Source:** `AmountMST balancePerDate(TransDate _transactionDate = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        return this.CustVendTable::balanceMST(dateNull(), _transactionDate);
    }`
    - **Method:**
      - **Name:** `bankAccountNum`
      - **Source:** `display CustomerBankAccount bankAccountNum()
    {
        CustomerBankAccount bankAccount;
        SecurityRights  sr;
        SecurityTableRights stRights;
        AccessRight ar;

        sr = SecurityRights::construct();
        stRights = sr.tableFieldAccessRights('CustBankAccount');
        ar = stRights.fieldAccessRight('AccountNum');

        unchecked(Uncheck::TableSecurityPermission)
        {
            // Only make database call when the AccountNum and bankAccount is not empty.
            if (ar != AccessRight::NoAccess && this.AccountNum != '' && this.BankAccount != '')
            {
                bankAccount = CustBankAccount::find(this.AccountNum, this.BankAccount).AccountNum;
            }
        }

        return bankAccount;
    }`
    - **Method:**
      - **Name:** `registrationNumber`
      - **Source:** `display VATNum registrationNumber()
    {
        return TaxRegistration::getPrimaryRegistrationNumber(this, TaxRegistrationTypesList::TAXID);
    }`
    - **Method:**
      - **Name:** `canDisplayCreditLimit`
      - **Source:** `/// <summary>
    ///    Determines whether the credit limit can be displayed.
    /// </summary>
    /// <returns>
    ///    true if the credit limit can be displayed; otherwise, false.
    /// </returns>
    /// <remarks>
    ///    The determination is based on whether a meaningful value has been entered.
    /// </remarks>
    public boolean canDisplayCreditLimit()
    {
        boolean canDisplay;

        canDisplay =
            (this.CreditMax != 0) &&
            (this.Currency != '') &&
            (this.Currency == CompanyInfoHelper::standardCurrency());

        return canDisplay;
    }`
    - **Method:**
      - **Name:** `cashDiscName`
      - **Source:** `display CustCashDiscName cashDiscName()
    {
        return CashDisc::find(this.CashDisc).Description;
    }`
    - **Method:**
      - **Name:** `checkAccountBlocked`
      - **Source:** `boolean checkAccountBlocked(AmountCur _amountCur)
    {
        boolean ret = true;

        if (this.Blocked    == CustVendorBlocked::All ||
           (this.Blocked    == CustVendorBlocked::Invoice && _amountCur > 0))
        {
            ret = checkFailed(strFmt("@SYS7987", this.AccountNum));
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `checkBlockedAll`
      - **Source:** `boolean checkBlockedAll()
    {
        boolean ret = true;

        if (this.Blocked == CustVendorBlocked::All)
        {
            ret = checkFailed(strFmt("@SYS18389", this.AccountNum, CustVendorBlocked::All));
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `checkRelatedAgreement_RU`
      - **Source:** `/// <summary>
    /// Validates if intercompany agreement exists.
    /// </summary>
    /// <returns>
    /// false if related agreement exists.
    /// </returns>
    public boolean checkRelatedAgreement_RU()
    {
        SalesAgreementHeader salesAgreementHeader;
        PurchAgreementHeader purchAgreementHeader;
        boolean              checkOk = true;

        setPrefix("@GLS115652");

        while select salesAgreementHeader
            where salesAgreementHeader.CustAccount        == this.AccountNum
               && salesAgreementHeader.SellingLegalEntity == CompanyInfo::current()
        {
            purchAgreementHeader = salesAgreementHeader.purchAgreementHeader_RU();

            checkOk = checkFailed(strFmt("@GLS115653",
                                         salesAgreementHeader.SalesNumberSequence,
                                         salesAgreementHeader.CustAccount,
                                         purchAgreementHeader.PurchNumberSequence,
                                         purchAgreementHeader.VendAccount));
        }

        return checkOk;
    }`
    - **Method:**
      - **Name:** `checkVATNumUsed`
      - **Source:** `boolean checkVATNumUsed()
    {
        boolean    ret = true;
        CustTable  tmpCustTable;

        #ISOCountryRegionCodes

        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoIT]) && this.VATNum)
        {
            select firstonly RecId from tmpCustTable
                where tmpCustTable.RecId  != this.RecId  &&
                      tmpCustTable.vatNum == this.vatNum;
            if (tmpCustTable.RecId)
            {
                ret = checkFailed(strFmt("@SYS79490",fieldPName(CustTable, vatNum)));
            }
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `isSameCustomer`
      - **Source:** `[Hookable(false)]
    public static boolean isSameCustomer(AccountNum _accountNumFirst, DataAreaId _dataAreaIdFirst, AccountNum _accountNumSecond, DataAreaId _dataAreaIdSecond)
    {
        CustTable custTableFirst = CustTable::findByCompany(_dataAreaIdFirst, _accountNumFirst);
        CustTable custTableSecond = CustTable::findByCompany(_dataAreaIdSecond, _accountNumSecond);

        return custTableFirst && custTableSecond && custTableFirst.Party == custTableSecond.Party;
    }`
    - **Method:**
      - **Name:** `cityName_BR`
      - **Source:** `/// <summary>
    ///     Gets the city name based on the customer primary postal address
    /// </summary>
    /// <returns>
    ///     The city name
    /// </returns>
    display LogisticsAddressCityName cityName_BR()
    {
        return this.postalAddress().City;
    }`
    - **Method:**
      - **Name:** `clearingLedgerDimension`
      - **Source:** `public LedgerDimensionDefaultAccount clearingLedgerDimension(CustPostingProfile _postingProfile = CustParameters::find().PostingProfile, CustVendAC  _accountNum = this.AccountNum)
    {
        return this.CustVendTable::clearingLedgerDimension(_postingProfile, _accountNum);
    }`
    - **Method:**
      - **Name:** `clearingLedgerDimensionGroup`
      - **Source:** `public LedgerDimensionDefaultAccount clearingLedgerDimensionGroup(
        CustAccount         _custAccount,
        CustPostingProfile  _custPostingProfile = CustParameters::find().PostingProfile)
    {
        CustLedgerAccounts  custLedgerAccounts;

        return CustVendLedgerDimensions::clearingLedgerDimension(_custAccount,
                                                       _custPostingProfile,
                                                       custLedgerAccounts,
                                                       TableGroupAll::GroupId);
    }`
    - **Method:**
      - **Name:** `clearingPeriod`
      - **Source:** `ClearingPeriod clearingPeriod()
    {
        return (this.ClearingPeriod ? this.ClearingPeriod : CustGroup::find(this.CustGroup).clearingPeriod());
    }`
    - **Method:**
      - **Name:** `companyCountryRegionISOCode`
      - **Source:** `/// <summary>
    /// Retrieves the <c>LogisticsAddressCountryRegionISOCode</c> code associated with the current record.
    /// </summary>
    /// <returns>
    /// The <c>LogisticsAddressCountryRegionISOCode</c> code.
    /// </returns>
    display LogisticsAddressCountryRegionISOCode companyCountryRegionISOCode()
    {
        return SysCountryRegionCode::countryInfo(this.DataAreaId);
    }`
    - **Method:**
      - **Name:** `companyInfo`
      - **Source:** `/// <summary>
    /// Retrieves the <c>CompanyInfo</c> record associated with the current record.
    /// </summary>
    /// <returns>
    /// The <c>CompanyInfo</c> record.
    /// </returns>
    public CompanyInfo companyInfo()
    {
        return CompanyInfo::findDataArea(this.company());
    }`
    - **Method:**
      - **Name:** `convertCurrencyCode`
      - **Source:** `void convertCurrencyCode()
    {
        CurrencyCode        origCurrencyCode;
        SalesTable          salesTable;
        CustInvoiceTable    custInvoiceTable;

        origCurrencyCode = this.orig().Currency;

        ttsbegin;

        salesTable  = SalesTable::custOpenOrders(this.AccountNum,true);

        while (salesTable)
        {
            if (salesTable.CurrencyCode == origCurrencyCode)
            {
                salesTable.convertCurrencyCode(this.Currency);
                salesTable.update();
            }
            next salesTable;
        }

        custInvoiceTable = CustInvoiceTable::custOpenInvoices(this.AccountNum,true);

        while (custInvoiceTable)
        {
            if (custInvoiceTable.CurrencyCode == origCurrencyCode)
            {
                custInvoiceTable.convertCurrencyCode(this.Currency);
                custInvoiceTable.update();
            }
            next custInvoiceTable;
        }

        ttscommit;
    }`
    - **Method:**
      - **Name:** `copyDimension`
      - **Source:** `/// <summary>
    /// Retrieves a dimension set that can be applied to the <c>defaultDimension</c> field on this table.
    /// </summary>
    /// <param name="_defaultDimension">
    /// A dimension set to apply to the <c>defaultDimension</c> field on this table.
    /// </param>
    /// <param name="_dimensionCopy">
    /// A <c>dimensionCopy</c> object that was previously initialized by using the current buffer; optional.
    /// </param>
    /// <returns>
    /// A dimension set that can be applied to the <c>defaultDimension</c> field on this table.
    /// </returns>
    /// <remarks>
    /// This method makes sure that potential linked dimensions are not overwritten.
    /// </remarks>

    public DimensionDefault copyDimension(
        DimensionDefault _defaultDimension,
        DimensionCopy    _dimensionCopy = DimensionCopy::newFromTable(this,
                                                                      this.companyInfo().RecId
                                                                      )
        )
    {
        return _dimensionCopy.copy(_defaultDimension);
    }`
    - **Method:**
      - **Name:** `copyInfoFromParty`
      - **Source:** `private void copyInfoFromParty()
    {
        Set sourceFieldGroups = new Set(Types::Class);
        sourceFieldGroups.add(new SysDictFieldGroup(tableNum(CustTable), tableFieldgroupStr(CustTable, MexicanSharedInfoByParty)));
        sourceFieldGroups.add(new SysDictFieldGroup(tableNum(VendTable), tableFieldgroupStr(VendTable, MexicanSharedInfoByParty)));

        SysDictFieldGroup targetFieldGroup = new SysDictFieldGroup(tableNum(CustTable), tableFieldgroupStr(CustTable, MexicanSharedInfoByParty));

        DirPartyInformationCopy::copyInfoFromEntitiesThatShareSameParty(this.Party, sourceFieldGroups, targetFieldGroup, this, false);
    }`
    - **Method:**
      - **Name:** `copyInfoToParty`
      - **Source:** `private void copyInfoToParty()
    {
        SysDictFieldGroup sourceFieldGroup = new SysDictFieldGroup(tableNum(CustTable), tableFieldgroupStr(CustTable, MexicanSharedInfoByParty));

        Set targetFieldGroups = new Set(Types::Class);
        targetFieldGroups.add(new SysDictFieldGroup(tableNum(CustTable), tableFieldgroupStr(CustTable, MexicanSharedInfoByParty)));
        targetFieldGroups.add(new SysDictFieldGroup(tableNum(VendTable), tableFieldgroupStr(VendTable, MexicanSharedInfoByParty)));

        DirPartyInformationCopy::copyInfoToEntitiesThatShareSameParty(this.Party, sourceFieldGroup, this, targetFieldGroups);
    }`
    - **Method:**
      - **Name:** `countryName`
      - **Source:** `display LogisticsAddressCountryRegionShortName countryName()
    {
        LogisticsPostalAddress postalAddress = this.postalAddress();

        return LogisticsAddressCountryRegionTranslation::find(postalAddress.CountryRegionId).ShortName;
    }`
    - **Method:**
      - **Name:** `countryRegionId`
      - **Source:** `/// <summary>
    /// Retrieves the <c>LogisticsAddressCountryRegionId</c> code associated with the current record.
    /// </summary>
    /// <returns>
    /// The <c>LogisticsAddressCountryRegionId</c> code.
    /// </returns>
    LogisticsAddressCountryRegionId countryRegionId()
    {
        return this.postalAddress().CountryRegionId;
    }`
    - **Method:**
      - **Name:** `countyName`
      - **Source:** `display LogisticsAddressCountyName countyName()
    {
        LogisticsPostalAddress postalAddress = this.postalAddress();

        return LogisticsAddressCounty::find(postalAddress.CountryRegionId, postalAddress.State, postalAddress.County).Name;
    }`
    - **Method:**
      - **Name:** `creditMaxCur`
      - **Source:** `display CustCreditMaxCur creditMaxCur()
    {
        return CurrencyExchangeHelper::curAmount(this.CreditMax, this.Currency);
    }`
    - **Method:**
      - **Name:** `currencyName`
      - **Source:** `display CurrencyName currencyName()
    {
        return Currency::find(this.Currency).Txt;
    }`
    - **Method:**
      - **Name:** `custCNPJ_BR`
      - **Source:** `display CNPJCPFNum_BR custCNPJ_BR()
    {
        if (!hasFieldAccess(tableNum(CustTable), fieldNum(CustTable, cnpjcpfNum_BR ), AccessType::View))
        {
            throw error("@SYS57330"); // Insufficient rights
        }

        return strAlpha(CustTable::findRecId(this.RecId).cnpjcpfNum_BR);
    }`
    - **Method:**
      - **Name:** `customerGroup`
      - **Source:** `display smmCustomerGroup customerGroup()
    {
        return  CustGroup::find(this.CustGroup).Name;
    }`
    - **Method:**
      - **Name:** `delete`
      - **Source:** `void delete()
    {
        PdsBatchAttribByItemCustomer    pdsBatchAttribByItemCustomer;

        // Check to see if the associated dimension attribute value has been used
        // in a way that would prevent deletion.
        if (!DimensionValidation::canDeleteEntityValue(this))
        {
            throw error(strFmt("@SYS134392", this.AccountNum));
        }

        ttsbegin;

        // Update the associated dimension attribute value.
        DimensionAttributeValue::updateForEntityValueDelete(this);

        // Delete the business relation
        smmBusRelTable::deleteFromCustVend(this.TableId,this.Party);

        // Remove links to contact person
        ContactPerson::removeCustVendLink(this.TableId, this.AccountNum);

        InventPosting::deleteFromCust(this.AccountNum);
        if (CustParameters::find().SalesCycle)
        {
            SalesPurchCycle::deleteFromCust(this.AccountNum);
        }

        super();

        delete_from pdsBatchAttribByItemCustomer
            where   pdsBatchAttribByItemCustomer.PdsBatchAttribAccountCode      == TableGroupAll::Table
            &&      pdsBatchAttribByItemCustomer.PdsBatchAttribAccountRelation  == this.AccountNum;
        DirPartyRelationship::removeLegalEntityRelationship(this.Party, DirSystemRelationshipType::Customer);

        // Auto delete party at backend
        DirParty::autoDeleteParty(this.Party);

        smmTransLog::initTrans(this, smmLogAction::delete);

        TaxBusinessService::deleteTaxIdentificationDimension(this.TableId, this.RecId);
        
        // Clean up SpecTrans for Customer being deleted
        SpecTransManager manager = SpecTransManager::newFromSpec(this, false);
        manager.deleteAll();
        ttscommit;
    }`
    - **Method:**
      - **Name:** `diffCurrencyCodes`
      - **Source:** `container diffCurrencyCodes()
    {
        CustTrans   custTranslocal;
        container   currencyCodes;

        while select CurrencyCode from custTranslocal
            group by CurrencyCode
            where custTranslocal.AccountNum == this.AccountNum
        {
            currencyCodes += custTranslocal.CurrencyCode;
        }

        return currencyCodes;
    }`
    - **Method:**
      - **Name:** `diffCurrencyCodesOnOpen`
      - **Source:** `container diffCurrencyCodesOnOpen(TransDate _transactionDate = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        CustTrans   custTranslocal;
        container   currencyCodes;

        while select CurrencyCode from custTranslocal
            group by CurrencyCode
            where custTranslocal.AccountNum == this.AccountNum
        {
            if (this.balanceCurPerDate(_transactionDate, custTranslocal.CurrencyCode))
            {
                currencyCodes += custTranslocal.CurrencyCode;
            }
        }

        return currencyCodes;
    }`
    - **Method:**
      - **Name:** `districtName_BR`
      - **Source:** `/// <summary>
    ///     Gets the district name based on the customer primary postal address
    /// </summary>
    /// <returns>
    ///     The district name
    /// </returns>
    display LogisticsAddressDistrictName districtName_BR()
    {
        return this.postalAddress().DistrictName;
    }`
    - **Method:**
      - **Name:** `editContactPersonName`
      - **Source:** `edit CustContactPersonName editContactPersonName(boolean _set, ContactPersonName _name)
    {
        return this.CustVendTable::editContactPersonName(_set, _name);
    }`
    - **Method:**
      - **Name:** `email`
      - **Source:** `display Email email()
    {
        LogisticsElectronicAddress electronicAddress;

        electronicAddress = DirParty::primaryElectronicAddress(this.Party, LogisticsElectronicAddressMethodType::Email);

        if (electronicAddress)
        {
            Email email = electronicAddress.Locator;
            electronicAddress.dispose();
            return email;
        }

        return '';
    }`
    - **Method:**
      - **Name:** `endDiscName`
      - **Source:** `display EndDiscName endDiscName()
    {
        return PriceDiscGroup::find(ModuleInventCustVend::Cust,
                                    PriceGroupType::EndDiscGroup,
                                    this.EndDisc).Name;
    }`
    - **Method:**
      - **Name:** `exceedMax`
      - **Source:** `AmountMST exceedMax(Amount _nowBalance)
    {
        if (this.CreditMax)
        {
            return max(0, _nowBalance - this.CreditMax);
        }
        else
        {
            return 0;
        }
    }`
    - **Method:**
      - **Name:** `existOpenOrders`
      - **Source:** `boolean existOpenOrders()
    {
        return (SalesTable::existCustOpenOrder(this.AccountNum) || CustInvoiceTable::existCustOpenInvoice(this.AccountNum));
    }`
    - **Method:**
      - **Name:** `firstOpenPayment`
      - **Source:** `CustVendTransOpen firstOpenPayment()
    {
        CustTransOpen   custTransOpen;

        custTransOpen = this.CustVendTable::firstOpenPayment();

        return custTransOpen;
    }`
    - **Method:**
      - **Name:** `firstSettledPayment`
      - **Source:** `CustVendSettlement firstSettledPayment()
    {
        CustSettlement  custSettlement;

        custSettlement = this.CustVendTable::firstSettledPayment();

        return custSettlement;
    }`
    - **Method:**
      - **Name:** `freeValueCur`
      - **Source:** `display CustCreditFreeValueCur freeValueCur(AmountMST _adjustment = 0)
    {
        return this.CustVendTable::freeValueCur(_adjustment);
    }`
    - **Method:**
      - **Name:** `freeValueMST`
      - **Source:** `display CustCreditFreeValueMST freeValueMST(AmountMST _adjustment = 0)
    {
        return this.CustVendTable::freeValueMST(_adjustment);
    }`
    - **Method:**
      - **Name:** `getCurrenciesForAllTrans`
      - **Source:** `/// <summary>
    /// Gets the list of currencies for the transactions in the specified date range.
    /// </summary>
    /// <param name="_startDate">
    /// The date of the first transactions to be included.
    /// </param>
    /// <param name="_endDate">
    /// The date of the last transactions to be included.
    /// </param>
    /// <param name="_includeReversedTrans">
    /// A Boolean value that indicates whether to include reversed transactions.
    /// </param>
    /// <returns>
    /// A list of currencies.
    /// </returns>
    /// <remarks>
    /// Exchange adjustment transactions are not included. A unique currency is
    /// only included in the list once.
    /// </remarks>
    public container getCurrenciesForAllTrans(
        TransDate _startDate,
        TransDate _endDate,
        boolean _includeReversedTrans)
    {
        return this.getCurrenciesForTrans(_startDate, _endDate, _includeReversedTrans, false);
    }`
    - **Method:**
      - **Name:** `getCurrenciesForOpenTrans`
      - **Source:** `/// <summary>
    /// Gets the list of currencies for the open transactions in the specified date range.
    /// </summary>
    /// <param name="_startDate">
    /// The date of the first open transactions to be included.
    /// </param>
    /// <param name="_endDate">
    /// The date of the last open transactions to be included.
    /// </param>
    /// <param name="_includeReversedTrans">
    /// A Boolean value that indicates whether to include reversed transactions.
    /// </param>
    /// <returns>
    /// A list of currencies.
    /// </returns>
    /// <remarks>
    /// Exchange adjustment transactions are not included. A unique currency is
    /// only included in the list once.
    /// </remarks>
    public container getCurrenciesForOpenTrans(
        TransDate _startDate,
        TransDate _endDate,
        boolean _includeReversedTrans)
    {
        return this.getCurrenciesForTrans(_startDate, _endDate, _includeReversedTrans, true);
    }`
    - **Method:**
      - **Name:** `getCurrenciesForTrans`
      - **Source:** `/// <summary>
    /// Gets the list of currencies for the transactions in the specified date range.
    /// </summary>
    /// <param name="_startDate">
    /// The date of the first transactions to be included.
    /// </param>
    /// <param name="_endDate">
    /// The date of the last transactions to be included.
    /// </param>
    /// <param name="_includeReversedTrans">
    /// A Boolean value that indicates whether to include reversed transactions.
    /// </param>
    /// <param name="_openOnly">
    /// A Boolean value that indicates whether to only include open transactions.
    /// </param>
    /// <returns>
    /// A list of currencies.
    /// </returns>
    /// <remarks>
    /// Exchange adjustment transactions are not included. A unique currency is
    /// only included in the list once.
    /// </remarks>
    private container getCurrenciesForTrans(
        TransDate _startDate,
        TransDate _endDate,
        boolean _includeReversedTrans,
        boolean _openOnly = false)
    {
        CustTrans               trans;
        Query                   q = new Query();
        QueryRun                run;
        QueryBuildDataSource    transDataSource;
        QueryBuildDataSource    transOpenDataSource;
        QueryBuildDataSource    reversalDataSource;
        QueryBuildRange         range;
        container               currencyList;

        Debug::assert(this.AccountNum != '');
        Debug::assert(_startDate <= _endDate);

        transDataSource = q.addDataSource(tableNum(CustTrans));
        transDataSource.addGroupByField(fieldNum(CustTrans, CurrencyCode));

        range = transDataSource.addRange(fieldNum(CustTrans, AccountNum));
        range.value(SysQuery::value(this.AccountNum));

        range = transDataSource.addRange(fieldNum(CustTrans, TransDate));
        range.value(SysQuery::range(_startDate, _endDate));

        range = transDataSource.addRange(fieldNum(CustTrans, TransType));
        range.value(SysQuery::valueNot(LedgerTransType::ExchAdjustment));

        if (_openOnly)
        {
            transOpenDataSource = transDataSource.addDataSource(tableNum(CustTransOpen));
            transOpenDataSource.joinMode(JoinMode::ExistsJoin);
            transOpenDataSource.addLink(fieldNum(CustTrans, RecId), fieldNum(CustTransOpen, RefRecId));
            transOpenDataSource.addLink(fieldNum(CustTrans, AccountNum), fieldNum(CustTransOpen, AccountNum));
        }

        if (!_includeReversedTrans)
        {
            reversalDataSource = transDataSource.addDataSource(tableNum(TransactionReversalTrans));
            reversalDataSource.joinMode(JoinMode::NoExistsJoin);
            reversalDataSource.addLink(fieldNum(CustTrans, TableId), fieldNum(TransactionReversalTrans, RefTableId));
            reversalDataSource.addLink(fieldNum(CustTrans, RecId), fieldNum(TransactionReversalTrans, RefRecId));
            reversalDataSource.addRange(fieldNum(TransactionReversalTrans, Reversed)).value(enum2str(NoYes::Yes));
        }

        run = new QueryRun(q);
        while (run.next())
        {
            trans = run.get(tableNum(CustTrans)) as CustTrans;

            currencyList += trans.CurrencyCode;
        }

        return currencyList;
    }`
    - **Method:**
      - **Name:** `getSettleDate`
      - **Source:** `public TransDate getSettleDate(SettleDatePrinc    _saveDatePrinciple      = SettleDatePrinc::DateOfPayment,
                                   TransDate          _saveDate               = dateNull())
    {
        return this.CustVendTable::getSettleDate(_saveDatePrinciple, _saveDate);
    }`
    - **Method:**
      - **Name:** `getStructDepartment_RU`
      - **Source:** `/// <summary>
    /// Implementation of \Data Dictionary\Maps\CustVendTable\Methods\getStructDepartment_RU
    /// </summary>
    /// <returns>
    /// Returns current structure department
    /// </returns>
    public StructDepartment_RU getStructDepartment_RU()
    {
        return "";
    }`
    - **Method:**
      - **Name:** `getTaxInformationCustTable_IN`
      - **Source:** `/// <summary>
    /// get TaxInformation of customer.
    /// </summary>
    /// <param name="_forUpdate">
    /// whether the record can be update.
    /// </param>
    /// <returns>
    /// the customer taxInformation transaction.
    /// </returns>
    public TaxInformationCustTable_IN getTaxInformationCustTable_IN(boolean  _forUpdate = false)
    {
        TaxInformationCustTable_IN taxInformationCustTable_IN;

        taxInformationCustTable_IN.selectForUpdate(_forUpdate);

        select firstonly taxInformationCustTable_IN
            where taxInformationCustTable_IN.CustTable == this.AccountNum;

        return taxInformationCustTable_IN;
    }`
    - **Method:**
      - **Name:** `getTaxWithholdGroup`
      - **Source:** `/// <summary>
    /// Gets the withholding tax group of current customer.
    /// </summary>
    /// <returns>
    /// The withholding tax group.
    /// </returns>
    /// <remarks>
    /// For Thailand, if the "Calculate withholding tax" is not checked, then always return empty.
    /// </remarks>
    public TaxWithholdGroup getTaxWithholdGroup()
    {
        if (TaxThaiGovCertificationFeatureChecker::isTaxWithholdEnabled()
            && this.TaxWithholdCalculate_TH == NoYes::No)
        {
            return '';
        }
        else
        {
            return this.TaxWithholdGroup_TH;
        }
    }`
    - **Method:**
      - **Name:** `hasForecastSales`
      - **Source:** `display ForecastHasSales hasForecastSales()
    {
        return (select forecastSales
                    where forecastSales.CustAccountId == this.AccountNum).RecId != 0;
    }`
    - **Method:**
      - **Name:** `initFromCustGroup`
      - **Source:** `void initFromCustGroup(CustGroup _custGroup)
    {
        // <GEEPL>
        #isoCountryRegionCodes
        // </GEEPL>
        //
        //If Terms of Payment is set on customer group
        //then only Terms of Payment on customers is filled in by default from PaymTermId
        //
        if (_custGroup.PaymTermId)
        {
            this.PaymTermId = _custGroup.PaymTermId;
        }

        this.PaymSched  = PaymTerm::find(this.PaymTermId).PaymSched;

        //
        //If default tax group is set on customer group
        //then sales tax group on customers is filled in by default from TaxGroupId
        //
        if (_custGroup.TaxGroupId)
        {
            this.TaxGroup = _custGroup.TaxGroupId;
        }

        //
        //If DefaultDimension is set on customer group
        //then financial dimensions on customers is filled in by default from customer financial dimensions
        //
        if (_custGroup.DefaultDimension)
        {
            this.DefaultDimension = _custGroup.DefaultDimension;
        }

        //
        // If default write-off account is set on the customer group
        // then write-off account on the customer is defaulted from the customer group
        //
        if (_custGroup.CustWriteOffRefRecId)
        {
            this.CustWriteOffRefRecId = _custGroup.CustWriteOffRefRecId;
        }

        // If default payment Id type is set on the customer group and payment ID feature is enabled
        // then payment Id type on the customer is defaulted from the customer group
        if (CustConfigurablePaymentIdFeature_CH::isEnabled() && _custGroup.BankCustPaymIdTable)
        {
            this.BankCustPaymIdTable = _custGroup.BankCustPaymIdTable;
        }

        //<GEEPL>
        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoPL]))
        {
            this.TaxPeriodPaymentCode_PL = _custGroup.TaxPeriodPaymentCode_PL?
                                        _custGroup.TaxPeriodPaymentCode_PL:
                                        CustParameters::find().TaxPeriodPaymentCode_PL;
        }
        //</GEEPL>

        //
        //Use the default PriceIncludesSalesTax value for the given CustomerGroup
        //
        this.InclTax = _custGroup.PriceIncludeSalesTax;
    }`
    - **Method:**
      - **Name:** `initFromsmmLeadTable`
      - **Source:** `void initFromsmmLeadTable()
    {
        smmLeadTable   smmLeadTable;
        select RecId, OwnerWorker from smmLeadTable where smmLeadTable.Party == this.Party;
        if (smmLeadTable.RecId)
        {
            this.MainContactWorker = smmLeadTable.OwnerWorker;
        }
    }`
    - **Method:**
      - **Name:** `initValue`
      - **Source:** `void initValue()
    {
        CustFormletterParameters custFormletterParameters;
        CompanyInfo companyInfo;
        //<GEERU>
        #ISOCountryRegionCodes
        //</GEERU>

        super();
        this.Party = 0;            // Reset PartyId, it might be set when templates are used

        // If the languageId and Currency are empty strings a blank new cutomer is created in the table and thus
        // the following values should be initialised with default values. If the languageId and Currency are non-empty
        // the customer is created from a template and the values should not be touched.
        if ( this.Currency == "")
        {
            companyInfo                     = CompanyInfo::find();
            this.Currency                   = Ledger::accountingCurrency(companyInfo.RecId);

            custFormletterParameters        = CustFormletterParameters::find();
            this.GiroType                   = custFormletterParameters.GiroOnInvoice;
            this.GiroTypeInterestNote       = custFormletterParameters.GiroOnInterest;
            this.GiroTypeCollectionletter   = custFormletterParameters.GiroOnCollectionletter;
            this.GiroTypeFreeTextInvoice    = custFormletterParameters.GiroOnFreeTextInvoice;
            this.GiroTypeProjInvoice        = ProjFormletterParameters::find().GiroOnInvoice;
            this.GiroTypeAccountStatement   = custFormletterParameters.GiroOnAccountStatement;
        }
        //<GEERU>
        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU]))
        {
            this.InvoicePostingType_RU = SalesParameters::find().InvoicePostingType_RU;
        }
        //<GEERU>

        this.MainContactWorker = smmUtility::getCurrentContactWorker();

        // <GBR>
        if (BrazilParameters::isEnabled())
        {
            this.ICMSContributor_BR = NoYes::Yes;
        }
        // </GBR>
    }`
    - **Method:**
      - **Name:** `insert`
      - **Source:** `void insert(DirPartyType _partyType = DirPartyType::None, Name _name = '',boolean _updateCRM=true)
    {
        DirPartyType   type;
        // <GEERU>
        #isoCountryRegionCodes
        // </GEERU>

        // <GBR>
        CustTable sourceCustTable;
        // </GBR>

        ttsbegin;

        // <GEERU>
        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU]))
        {
            this.setInventProfileId_RU();
        }
        // </GEERU>

        // Check if not associated to Party
        if (!this.Party)
        {
            // Create a Party entry for customer
            this.Party = DirPartyTable::createNew(_partyType, _name).RecId;
        }
        else
        {
            if (!this.VATNum && !CustParameters::find().CustTableCopyTaxRegistionNumAsTaxExemptNum)
            {
                this.VATNum = TaxRegistration::getPrimaryRegistrationNumber(DirPartyTable::findRec(this.Party), TaxRegistrationTypesList::TAXID);
            }
            this.initFromsmmLeadTable();
        }

        if (this.Party)
        {
            // <GBR>
            if (BrazilParameters::isEnabled() && this.partyType() == DirPartyType::Person)
            {
                this.GenerateIncomingFiscalDocument_BR = NoYes::Yes;
            }
            // </GBR>
        }

        smmBusRelTable smmBusRelTable = smmBusRelTable::findByParty(this.Party, true);

        // B2B prospects should remain with type B2B prospect even after being approved as a customer.
        // So we need to skip the logic below if the customer being created is related to a B2B prospect.
        if (smmBusRelTable.RecId && !smmBusRelTable.isB2BProspect())
        {
            smmBusRelTypeId origBusRelTypeId = smmBusRelTable.BusRelTypeId;

            smmBusRelTable.BusRelTypeId = smmBusRelTypeGroup::getCustomerType();
            
            boolean updateProspectReferences = smmBusRelTable.updateProspectFromCustomer(this);

            super();

            if (updateProspectReferences)
            {
                smmBusRelTable.updateReferencesForConvertedProspect(this, origBusRelTypeId);
            }
        }
        else
        {
            super();
        }

        // Insert new customer in full text search table
        MCRFullTextSearch::insert(this);

        this.SysExtensionSerializerMap::postInsert();

        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
        {
            this.copyInfoToParty();
        }

        // <GBR>
        if (BrazilParameters::isEnabled())
        {
            select crossCompany * from sourceCustTable
                where sourceCustTable.Party == this.Party;

            FiscalInformationCopy_BR::copyFiscalInfoToCustVend(sourceCustTable);
        }
        // </GBR>

        smmTransLog::initTrans(this, smmLogAction::insert);

        DirPartyRelationship::createLegalEntityRelationship(this.Party, this.DataAreaId, DirSystemRelationshipType::Customer);

        // Add links to contact person
        ContactPerson::addCustVendLink(this.TableId, this.Party, this.AccountNum);

        // Create default location if using existing party
        LogisticsLocationDefaultAppUtil::createDefaultForExistingParty(this);
                
        DimensionDefaultFacade::copyDimensionValueToDefaultDimensionField(this, fieldNum(CustTable, AccountNum), this, fieldNum(CustTable, DefaultDimension));

        RetailEventNotificationAction::customerCreationCompletion(this.RecId);

        ttscommit;
    }`
    - **Method:**
      - **Name:** `interCompanyTradingPartner`
      - **Source:** `/// <summary>
    /// Finds the specified record in the <c>InterCompanyTradingPartner</c> table base on the current customer.
    /// </summary>
    /// <returns>
    /// A record in the <c>InterCompanyTradingPartner</c> table; otherwise, an empty record.
    /// </returns>
    public InterCompanyTradingPartner interCompanyTradingPartner()
    {
        return InterCompanyTradingPartner::findCustomer(this.Party,
                                                        this.company()
                                                        );
    }`
    - **Method:**
      - **Name:** `interCompanyTradingPartnerAccount`
      - **Source:** `/// <summary>
    /// Finds the account ID for the corresponding vendor for the current customer intercompany trading
    /// relation.
    /// </summary>
    /// <returns>
    /// A <c>VendAccount</c> object for the corresponding vendor intercompany trading partner.
    /// </returns>
    display VendAccount interCompanyTradingPartnerAccount()
    {
        InterCompanyTradingPartner  interCompanyTradingPartnerVendor;
        InterCompanyTradingRelation interCompanyTradingRelation = this.interCompanyTradingPartner().interCompanyTradingRelation();

        if (interCompanyTradingRelation)
        {
            interCompanyTradingPartnerVendor = InterCompanyTradingPartner::find(interCompanyTradingRelation.InterCompanyTradingVendor);
        }

        return interCompanyTradingPartnerVendor.vendTable().AccountNum;
    }`
    - **Method:**
      - **Name:** `interCompanyTradingPartnerCompanyID`
      - **Source:** `/// <summary>
    /// Finds the company ID for the corresponding vendor for the current customer intercompany trading relation.
    /// </summary>
    /// <returns>
    /// A company ID for the corresponding vendor intercompany trading partner.
    /// </returns>
    display InterCompanyCompanyId interCompanyTradingPartnerCompanyID()
    {
        InterCompanyTradingPartner  interCompanyTradingPartnerVendor;
        InterCompanyTradingRelation interCompanyTradingRelation = this.interCompanyTradingPartner().interCompanyTradingRelation();

        if (interCompanyTradingRelation)
        {
            interCompanyTradingPartnerVendor = InterCompanyTradingPartner::find(interCompanyTradingRelation.InterCompanyTradingVendor);
        }

        return interCompanyTradingPartnerVendor.VendorDataAreaId;
    }`
    - **Method:**
      - **Name:** `interCompanyTradingRelationActive`
      - **Source:** `/// <summary>
    /// Checks whether the current customer is in an active intercompany trading relation.
    /// </summary>
    /// <returns>
    /// true if the customer is in an active intercompany trading relation; otherwise, false.
    /// </returns>
    display InterCompanyTradingActive interCompanyTradingRelationActive()
    {
        InterCompanyTradingRelation interCompanyTradingRelation = this.interCompanyTradingPartner().interCompanyTradingRelation();

        return (interCompanyTradingRelation && interCompanyTradingRelation.Active);
    }`
    - **Method:**
      - **Name:** `invoiceAccountName`
      - **Source:** `display CustInvoiceAccountName invoiceAccountName()
    {
        return CustTable::find(this.InvoiceAccount).name();
    }`
    - **Method:**
      - **Name:** `invoiceAddress`
      - **Source:** `LogisticsPostalAddress invoiceAddress()
    {
        LogisticsPostalAddress address = DirParty::postalAddress(this.Party, LogisticsLocationRoleType::Invoice);

        if (!address)
        {
            address = DirParty::primaryPostalAddress(this.Party);
        }
        return address;
    }`
    - **Method:**
      - **Name:** `isForeign`
      - **Source:** `/// <summary>
    /// Indicates whether a Customer is a Foreign.
    /// </summary>
    /// <returns>
    /// true if a Customer is a Foreign; otherwise, false.
    /// </returns>
    public NoYes isForeign()
    {
        return this.getTaxInformationCustTable_IN().IsForeign;
    }`
    - **Method:**
      - **Name:** `isPreferential`
      - **Source:** `/// <summary>
    /// Indicates whether a Customer is a Preferential.
    /// </summary>
    /// <returns>
    /// true if a Customer is a Preferential; otherwise, false.
    /// </returns>
    public NoYes isPreferential()
    {
        return this.getTaxInformationCustTable_IN().IsPreferential;
    }`
    - **Method:**
      - **Name:** `languageId`
      - **Source:** `LanguageId languageId()
    {
        return DirPartyTable::findRec(this.Party).LanguageId;
    }`
    - **Method:**
      - **Name:** `lastCollectionLetterJour`
      - **Source:** `CustCollectionLetterJour lastCollectionLetterJour()
    {
        CustCollectionLetterJour    custCollectionLetterJour;

        select firstonly custCollectionLetterJour
            order CollectionLetterNum desc
            where custCollectionLetterJour.AccountNum == this.AccountNum
            && custCollectionLetterJour.Canceled == dateNull();

        return custCollectionLetterJour;
    }`
    - **Method:**
      - **Name:** `lastInterestJournal`
      - **Source:** `CustInterestJour lastInterestJournal()
    {
        CustInterestJour    custInterestJour;

        select firstonly custInterestJour
            order InterestNote desc
            where custInterestJour.AccountNum == this.AccountNum;

        return custInterestJour;
    }`
    - **Method:**
      - **Name:** `lastInvoiceJournal`
      - **Source:** `CustInvoiceJour lastInvoiceJournal(NoYes _invoiceCustomer = NoYes::Yes)
    {
        CustInvoiceJour custInvoiceJour;

        select firstonly custInvoiceJour
        index hint InvoiceAccountIdx
        order by InvoiceDate desc, InvoiceId desc
        where custInvoiceJour.InvoiceAccount == this.AccountNum &&
              custInvoiceJour.InvoiceId;
        return custInvoiceJour;
    }`
    - **Method:**
      - **Name:** `lastInvoiceDate`
      - **Source:** `display InvoiceDate lastInvoiceDate()
    {
        return this.lastInvoiceJournal(NoYes::Yes).InvoiceDate;
    }`
    - **Method:**
      - **Name:** `lastPayment`
      - **Source:** `CustTrans lastPayment()
    {
        CustTrans custTrans;

        select firstonly custTrans
            index hint AccountDateIdx
            order by TransDate desc, Voucher desc
            where custTrans.AccountNum == this.AccountNum &&
                  custTrans.Invoice == '' &&
                  custTrans.AmountCur < 0 &&
                  custTrans.TransType == LedgerTransType::Payment;

        return custTrans;
    }`
    - **Method:**
      - **Name:** `lastPaymentDate`
      - **Source:** `display TransDate lastPaymentDate()
    {
        return this.lastPayment().TransDate;
    }`
    - **Method:**
      - **Name:** `lastProjInvoice`
      - **Source:** `ProjInvoiceJour lastProjInvoice()
    {
        ProjInvoiceJour projInvoiceJour;

        select firstonly projInvoiceJour
        index hint InvoiceAccountIdx
        order by InvoiceDate desc, ProjInvoiceId desc
        where projInvoiceJour.InvoiceAccount == this.AccountNum &&
              projInvoiceJour.ProjInvoiceId;

        return projInvoiceJour;
    }`
    - **Method:**
      - **Name:** `lastProjInvoiceDate`
      - **Source:** `display InvoiceDate lastProjInvoiceDate()
    {
        return this.lastProjInvoice().InvoiceDate;
    }`
    - **Method:**
      - **Name:** `lineDiscName`
      - **Source:** `display LineDiscName lineDiscName()
    {
        return PriceDiscGroup::find(ModuleInventCustVend::Cust,
                                    PriceGroupType::LineDiscGroup,
                                    this.LineDisc).Name;
    }`
    - **Method:**
      - **Name:** `mcrCustTable`
      - **Source:** `/// <summary>
    /// Recovers the retail customer record.
    /// </summary>
    /// <returns>
    /// A record of <c>MCRCustTable</c>
    /// </returns>
    public MCRCustTable mcrCustTable()
    {
        return this.SysExtensionSerializerMap::getExtensionTable(tableNum(MCRCustTable));
    }`
    - **Method:**
      - **Name:** `birthDate`
      - **Source:** `/// <summary>
    /// Get birth date for customer.
    /// </summary>
    /// <returns>Birth date for customer.</returns>
    public BirthDate birthDate()
    {
        return DirPerson::find(this.Party).birthDate();
    }`
    - **Method:**
      - **Name:** `mcrDefaultDeliveryPostalAddress`
      - **Source:** `/// <summary>
    /// Retrievest the postal address for the current <c>CustTable</c> record.
    /// </summary>
    /// <returns>
    /// The postal address for the current <c>CustTable</c> record.
    /// </returns>
    public LogisticsPostalAddress mcrDefaultDeliveryPostalAddress()
    {
        LogisticsLocationEntity location;

        location = LogisticsLocationEntity::constructFromLocationRecId(LogisticsLocationDefault::findSimpleDefaultByRoleType(this, LogisticsLocationRoleType::Delivery).RecId);

        return location.getPostalAddress();
    }`
    - **Method:**
      - **Name:** `mcrDisplayIsMerged`
      - **Source:** `/// <summary>
    /// Displays if the customer was merged.
    /// </summary>
    /// <returns>
    /// 'Yes' if the customer is merged; otherwise, ''.
    /// </returns>
    public display String15 mcrDisplayIsMerged()
    {
        if (this.mcrMergedParent != '')
        {
            return enum2str(NoYes::Yes);
        }

        return '';
    }`
    - **Method:**
      - **Name:** `mcrIsMerged`
      - **Source:** `/// <summary>
    /// Makes a small x image if the customer is merged.
    /// </summary>
    /// <returns>
    /// Returns the image number.
    /// </returns>
    public display MCRMergeIndicator mcrIsMerged()
    {
        if (this.mcrMergedParent != '')
        {
            return 11341;  // small X sign
        }

        return 11342;
    }`
    - **Method:**
      - **Name:** `mcrPackMCRCustTable`
      - **Source:** `/// <summary>
    /// Packs the <c>MCRCustTable</c> record into the extension field on the corresponding
    /// <c>CustTable</c> record.
    /// </summary>
    /// <param name="_mcrCustTable">
    /// The <c>MCRCustTable</c> record to be packed.
    /// </param>
    public void mcrPackMCRCustTable(MCRCustTable _mcrCustTable)
    {
        _mcrCustTable.CustTable = this.RecId;
        this.SysExtensionSerializerMap::packExtensionTable(_mcrCustTable);
    }`
    - **Method:**
      - **Name:** `mergeDimension`
      - **Source:** `/// <summary>
    /// Retrieves a dimension set that holds the merged combination of the current <c>defaultDimension</c>
    /// field on this table and the specified dimension set.
    /// </summary>
    /// <param name="_primaryDefaultDimension">
    /// A first dimension set to merge with the current <c>defaultDimension</c> field on this table.
    /// </param>
    /// <param name="_secondaryDefaultDimension">
    /// A second dimension set to merge with the current <c>defaultDimension</c> field on this table;
    /// optional
    /// </param>
    /// <param name="_dimensionMerge">
    /// A <c>dimensionMerge</c> object that was previously initialized by using the current buffer;
    /// optional.
    /// </param>
    /// <returns>
    /// A dimension set that holds the merged combination of the current <c>defaultDimension</c> field on
    /// this table and the specified dimension set.
    /// </returns>
    /// <remarks>
    /// This method makes sure that potential linked dimensions are not overwritten when merging. The
    /// defaulting behavior of the entity specifier will be used.
    /// </remarks>

    public DimensionDefault mergeDimension(
        DimensionDefault _primaryDefaultDimension,
        DimensionDefault _secondaryDefaultDimension = 0,
        DimensionMerge   _dimensionMerge = DimensionMerge::newFromTable(this,
                                                                        this.companyInfo().RecId
                                                                        )

        )
    {
        return _dimensionMerge.merge(_primaryDefaultDimension, _secondaryDefaultDimension);
    }`
    - **Method:**
      - **Name:** `modifiedField`
      - **Source:** `public void modifiedField(FieldId _fieldId)
    {
        DlvMode         dlvMode;

        #ISOCountryRegionCodes

        super(_fieldId);

        switch (_fieldId)
        {
            case fieldNum(CustTable, PaymTermId) :
                this.PaymSched = PaymTerm::find(this.PaymTermId).PaymSched;
                break;

            case fieldNum(CustTable, CustGroup) :
                this.initFromCustGroup(CustGroup::find(this.CustGroup));
                break;

            case fieldNum(CustTable, PaymMode) :
                this.CustVendTable::paymModeModified();
                break;

            case fieldNum(CustTable, BankCentralBankPurposeCode) :
                this.CustVendTable::modifiedBankCentralBankPurposeCode();
                break;
            case fieldNum(CustTable, InventSiteId) :
                this.InventStorageDimMap::modifiedField(fieldNum(InventStorageDimMap, InventSiteId));
                break;

            case fieldNum(CustTable, InventLocation) :
                this.InventStorageDimMap::modifiedField(fieldNum(InventStorageDimMap, InventLocationId));
                break;

            //shipping module
            case fieldNum(CustTable, DlvMode) :
                dlvMode = DlvMode::find(this.DlvMode);
                this.ShipCarrierId = dlvMode.ShipCarrierId;
                this.ShipCarrierAccountCode = dlvMode.ShipCarrierAccountCode;
                break;

            case fieldNum(CustTable, OrgId) :
                if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoIS]))
                {
                    if (this.RecId == 0
                        && (this.AccountNum == '' ||
                            Box::yesNo("@SYS113290", DialogButton::Yes) == DialogButton::Yes))
                    {
                        this.AccountNum = this.OrgId;
                    }
                    else
                    {
                        if (strCmp(this.OrgId, subStr(this.AccountNum, 1, strLen(this.OrgId))) != 0
                            && Box::yesNo(strFmt("@SYS113291", this.AccountNum), DialogButton::Yes) == DialogButton::Yes)
                        {
                            this.OrgId = this.orig().OrgId;
                        }
                    }
                }
                break;

            case fieldNum(CustTable, Party) :
                if (this.Party && !this.VendAccount && DirPartyTableHelper::isvendor(this.Party))
                {
                    this.VendAccount = VendTable::findByPartyRecId(this.Party).AccountNum;
                }

                if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
                {
                    this.copyInfoFromParty();
                }
                break;

            // <GEERU>
            case fieldNum(CustTable, InventProfileType_RU):
                if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU]))
                {
                    this.setInventProfileId_RU();
                }
                break;
            // </GEERU>

            case fieldNum(CustTable, CompanyType_MX) :
                this.Rfc_MX                 = '';
                this.Curp_MX                = '';
                this.StateInscription_MX    = '';
                break;

            case fieldNum(CustTable, EntryCertificateRequired_W) :
                if (this.EntryCertificateRequired_W == NoYes::No)
                {
                    this.IssueOwnEntryCertificate_W = NoYes::No;
                }
                break;

            case fieldNum(CustTable, EInvoiceRegister_IT) :
                this.EInvoice = this.EInvoiceRegister_IT;
                break;
        }

        // <GBR>
        if (BrazilParameters::isEnabled())
        {
            this.modifiedField_BR(_fieldId);
        }
        // </GBR>
    }`
    - **Method:**
      - **Name:** `modifiedField_BR`
      - **Source:** `/// <summary>
    /// Executes actions in Brazilian context depending on the field that was modified.
    /// </summary>
    /// <param name="_fieldId">
    /// The <c>fieldId</c> value of the field that was modified.
    /// </param>
    public void modifiedField_BR(fieldId _fieldId)
    {
        switch (_fieldId)
        {
            case fieldnum(CustTable, InclTax):
                if (this.InclTax)
                {
                    info("@GLS390");
                }
                break;

            case fieldnum(CustTable, INSSCEI_BR):
                this.INSSCEI_BR = FiscalInformationUtil_BR::formatINSSCEI(this.INSSCEI_BR);
                break;

            case fieldnum(CustTable, NIT_BR):
                this.NIT_BR = FiscalInformationUtil_BR::formatNIT(this.NIT_BR);
                break;

            case fieldNum(CustTable, Party):
                FiscalInformationCopy_BR::copyFiscalInfoFromCustVend(this);
                break;
        }
    }`
    - **Method:**
      - **Name:** `multiLineDiscName`
      - **Source:** `display MultiLineDiscName multiLineDiscName()
    {
        return PriceDiscGroup::find(ModuleInventCustVend::Cust,
                                    PriceGroupType::MultiLineDiscGroup,
                                    this.MultiLineDisc).Name;
    }`
    - **Method:**
      - **Name:** `name`
      - **Source:** `display CustName name()
    {
        DirPartyTable   dirPartyTable;
        CustName        custName;
        boolean         isSet = false;

        try
        {
            if (this.hasRelatedTable(identifierStr(DirPartyTable_FK)))
            {
                dirPartyTable = this.relatedTable(identifierStr(DirPartyTable_FK)) as DirPartyTable;

                //Check to make sure the fields we are accessing are selected.
                if (dirPartyTable && dirPartyTable.isFieldDataRetrieved(fieldStr(DirPartyTable, Name)))
                {
                    custName = dirPartyTable.Name;
                    isSet = true;
                }
            }
        }
        catch (Exception::Error)
        {
            isSet = false;
        }

        //If we aren't joined to DirPartyTable or it isn't selected, then do a query to get it.
        if (!isSet)
        {
            custName = DirPartyTable::getName(this.Party);
        }

        return custName;
    }`
    - **Method:**
      - **Name:** `nameAlias`
      - **Source:** `display NameAlias nameAlias()
    {
        DirPartyTable   dirPartyTable;
        NameAlias       nameAlias;
        boolean         isSet = false;

        if (this.hasRelatedTable(identifierStr(DirPartyTable_FK)))
        {
            dirPartyTable = this.relatedTable(identifierStr(DirPartyTable_FK));

            //Check to make sure the fields we are accessing are selected.
            if (dirPartyTable && dirPartyTable.isFieldDataRetrieved(fieldStr(DirPartyTable, NameAlias)))
            {
                //Check to make sure the fields we are accessing are selected.
                if (dirPartyTable.isFieldDataRetrieved(fieldStr(DirPartyTable, NameAlias)))
                {
                    nameAlias = dirPartyTable.NameAlias;
                    isSet = true;
                }
            }
        }

        //If we aren't joined to DirPartyTable or it isn't selected, then do a query to get it.
        if (!isSet)
        {
            nameAlias = DirPartyTable::findRec(this.Party).NameAlias;
        }

        return nameAlias;
    }`
    - **Method:**
      - **Name:** `openBalanceCur`
      - **Source:** `public AmountCur openBalanceCur(TransDate      _fromDate       = dateNull(),
                                    TransDate      _toDate         = dateMax(),
                                    TransDate      _assessmentDate = dateNull(),
                                    CurrencyCode   _currency       = CompanyInfoHelper::standardCurrency())
    {
        return this.CustVendTable::openBalanceCur(_fromDate, _toDate, _assessmentDate, _currency);
    }`
    - **Method:**
      - **Name:** `openBalanceMST`
      - **Source:** `display AmountMST openBalanceMST(FromDate   _fromDate       = dateNull(),
                                     ToDate     _toDate         = dateMax(),
                                     TransDate  _assessmentDate = dateNull())
    {
        return this.CustVendTable::openBalanceMST(_fromDate, _toDate, _assessmentDate);
    }`
    - **Method:**
      - **Name:** `openBalanceReportingCurrency`
      - **Source:** `display AmountMSTSecondary openBalanceReportingCurrency(FromDate   _fromDate       = dateNull(),
                                                            ToDate     _toDate         = dateMax(),
                                                            TransDate  _assessmentDate = dateNull())
    {
        return this.CustVendTable::openBalanceReportingCurrency(_fromDate, _toDate, _assessmentDate);
    }`
    - **Method:**
      - **Name:** `openBalanceMSTSecondary`
      - **Source:** `/// <summary>
    /// Display method to get the customer open balances in reporting currency.
    /// </summary>
    /// <param name = "_fromDate">From date; optional.</param>
    /// <param name = "_toDate">To date; optional.</param>
    /// <param name = "_assessmentDate">Assesment date; optional.</param>
    /// <returns>Returns the customer open balances in reporting currency.</returns>
    public display AmountMSTSecondary openBalanceMSTSecondary(FromDate   _fromDate       = dateNull(),
                                                       ToDate     _toDate         = dateMax(),
                                                       TransDate  _assessmentDate = dateNull())
    {
        return this.CustVendTable::openBalanceMSTSecondary(_fromDate, _toDate, _assessmentDate);
    }`
    - **Method:**
      - **Name:** `openBalanceMSTDoc`
      - **Source:** `AmountMST openBalanceMSTDoc(TransDate   _transDate  = dateNull(),
                                FromDate    _fromDate   = dateNull(),
                                ToDate      _toDate     = dateMax())
    {
        return this.CustVendTable::openBalanceMSTDoc(_transDate, _fromDate, _toDate);
    }`
    - **Method:**
      - **Name:** `openBalanceMSTDue`
      - **Source:** `AmountMST openBalanceMSTDue(TransDate   _transDate  = dateNull(),
                                FromDate    _fromDate   = dateNull(),
                                ToDate      _toDate     = dateMax())
    {
        return this.CustVendTable::openBalanceMSTDue(_transDate, _fromDate, _toDate);
    }`
    - **Method:**
      - **Name:** `openBalanceMSTPerAgreement_RU`
      - **Source:** `/// <summary>
    /// Calculate open customer balance with regards to contract (agreement)
    /// </summary>
    /// <param name="_agreementId">
    /// Agreement ID
    /// </param>
    /// <returns>
    /// Open customer balance with regards to contract (agreement)
    /// </returns>
    public AmountMST openBalanceMSTPerAgreement_RU(AgreementId_RU _agreementId)
    {
        return CustVendOpenBalancePerAgreementCalc_RU::construct(CustVendACType::Cust, this.AccountNum).calc(_agreementId);
    }`
    - **Method:**
      - **Name:** `openInvoiceBalanceMSTAssessmentDateQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening invoice balance.
    /// </summary>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>
    /// <param name = "_assessmentDate">
    /// Assassment date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openInvoiceBalanceMSTAssessmentDateQuery(
        FromDate   _fromDate,
        ToDate     _toDate,
        TransDate  _assessmentDate)
    {
        Query query = new Query();

        QueryBuildDataSource custSettlementQueryBuildDataSource = query.addDataSource(tableNum(CustSettlement));

        custSettlementQueryBuildDataSource.addSelectionField(fieldNum(CustSettlement, SettleAmountMST), SelectionField::Sum);
        custSettlementQueryBuildDataSource.addSelectionField(fieldNum(CustSettlement, ExchAdjustment), SelectionField::Sum);

        custSettlementQueryBuildDataSource.addGroupByField(fieldNum(CustSettlement, AccountNum));

        custSettlementQueryBuildDataSource.addRange(fieldNum(CustSettlement, AccountNum)).value(queryValue(this.AccountNum));

        custSettlementQueryBuildDataSource.addRange(fieldNum(CustSettlement, TransDate)).value('>' + SysQuery::value(_assessmentDate));

        QueryBuildDataSource custTransQueryBuildDataSource = custSettlementQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::InnerJoin);

        custTransQueryBuildDataSource.addLink(fieldNum(CustSettlement, TransRecId), fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, TransDate)).value(SysQuery::range(_fromDate, _toDate, true));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(
            strFmt('((%1.%2 != "") || ((%1.%2 == "") && (%1.%3 > 0)))',
                custTransQueryBuildDataSource.name(),
                fieldStr(CustTrans, Invoice),
                fieldStr(CustTrans, AmountMST)
            ));

        return query;
    }`
    - **Method:**
      - **Name:** `openInvoiceBalanceMSTQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening invoice balance.
    /// </summary>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openInvoiceBalanceMSTQuery(
        FromDate   _fromDate,
        ToDate     _toDate)
    {
        Query query = new Query();

        QueryBuildDataSource custTransOpenQueryBuildDataSource = query.addDataSource(tableNum(CustTransOpen));

        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, AmountMST), SelectionField::Sum);

        custTransOpenQueryBuildDataSource.addGroupByField(fieldNum(CustTransOpen, RefRecId));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, AccountNum)).value(queryValue(this.AccountNum));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, TransDate)).value(SysQuery::range(_fromDate, _toDate, true));

        QueryBuildDataSource custTransQueryBuildDataSource = custTransOpenQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::InnerJoin);
        custTransQueryBuildDataSource.addSelectionField(fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addLink(fieldNum(CustTransOpen, RefRecId), fieldNum(CustTrans, RecId));
        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(
            strFmt('((%1.%2 != "") || ((%1.%2 == "") && (%1.%3 > 0)))', 
                custTransQueryBuildDataSource.name(),
                fieldStr(CustTrans, Invoice),
                fieldStr(CustTrans, AmountMST)));

        return query;
    }`
    - **Method:**
      - **Name:** `openInvoiceBalanceMST`
      - **Source:** `display AmountMST openInvoiceBalanceMST(FromDate   _fromDate       = dateNull(),
                                     ToDate     _toDate         = dateMax(),
                                     TransDate  _assessmentDate = dateNull())
    {
        CustTrans       custTrans;
        CustTransOpen   custTransOpen;
        CustSettlement  custSettlement;
        AmountMST       openBalanceMST = 0;

        if (_assessmentDate)
        {
            if (!(hasFieldAccess(tableNum(CustSettlement), fieldNum(CustSettlement, SettleAmountMST) &&
                  hasFieldAccess(tableNum(CustSettlement), fieldNum(CustSettlement, ExchAdjustment)))))
            {
                throw error("@SYS57330");
            }

            QueryRun queryRun = new QueryRun(this.openInvoiceBalanceMSTAssessmentDateQuery(_fromDate, _toDate, _assessmentDate));

            if (queryRun.next())
            {
                custSettlement = QueryRun.get(tableNum(CustSettlement));
                openBalanceMST += custSettlement.SettleAmountMST + custSettlement.ExchAdjustment;
            }

            openBalanceMST += this.openInvoiceBalanceMST(_fromDate, _toDate);
        }
        else
        {
            if (!hasFieldAccess(tableNum(CustTransOpen), fieldNum(CustTransOpen, AmountMST)))
            {
                throw error("@SYS57330");
            }

            QueryRun queryRun = new QueryRun(this.openInvoiceBalanceMSTQuery(_fromDate, _toDate));
            while (queryRun.next())
            {
                custTransOpen = queryRun.get(tableNum(CustTransOpen));

                openBalanceMST += custTransOpen.AmountMST;
            }
        }

        return openBalanceMST;
    }`
    - **Method:**
      - **Name:** `openSalesInvoiceBalanceMSTWithCount`
      - **Source:** `internal container openSalesInvoiceBalanceMSTWithCount()
    {
        container custBalanceResponse = [0, 0];
        AmountMST openBalanceMST = 0;
        Counter openInvoiceCount = 0;

        CustInvoiceJour custInvoiceJour;
        CustTrans custTrans;
        CustTransOpen custTransOpen;

        select count(RecId)
            from custInvoiceJour
                where custInvoiceJour.InvoiceAccount == this.AccountNum
            join custTrans
                where custTrans.Invoice == custInvoiceJour.InvoiceId
                    && custTrans.AccountNum == custInvoiceJour.InvoiceAccount
                    && custTrans.TransDate == custInvoiceJour.InvoiceDate
                    && custTrans.Voucher == custInvoiceJour.LedgerVoucher
                    // Only consider sales order invoices
                    && custTrans.TransType == LedgerTransType::Sales
                    && custTrans.AmountCur > 0
            join sum(AmountMST) from custTransOpen
                where custTransOpen.AccountNum == custTrans.AccountNum
                    && custTransOpen.RefRecId == custTrans.RecId;

        openBalanceMST = custTransOpen.AmountMST;
        openInvoiceCount = custInvoiceJour.RecId;
  
        custBalanceResponse = [openBalanceMST, openInvoiceCount];
        return custBalanceResponse;
    }`
    - **Method:**
      - **Name:** `openInvoiceBalanceMSTDocQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening invoice balance.
    /// </summary>
    /// <param name = "_transDate">
    /// Transaction date value.
    /// </param>  
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>  
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openInvoiceBalanceMSTDocQuery(
        TransDate   _transDate,
        FromDate    _fromDate,
        ToDate      _toDate)
    {
        Query query = new Query();

        QueryBuildDataSource custTransOpenQueryBuildDataSource = query.addDataSource(tableNum(CustTransOpen));

        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, AmountMST), SelectionField::Sum);

        custTransOpenQueryBuildDataSource.addOrderByField(fieldNum(CustTransOpen, RefRecId));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, AccountNum)).value(queryValue(this.AccountNum));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, TransDate)).value(SysQuery::range(null, _transDate, true));

        QueryBuildDataSource custTransQueryBuildDataSource = custTransOpenQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::ExistsJoin);
        custTransQueryBuildDataSource.addSelectionField(fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addLink(fieldNum(CustTransOpen, RefRecId), fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, DocumentDate)).value(
            strFmt('(((%1.%2 != %6) && (%1.%2 >= %3) && (%1.%2 <= %4)) || ((%1.%2 == %6) && (%1.%5 >= %3) && (%1.%5 <= %4)))',
                custTransQueryBuildDataSource.name(),
                fieldStr(CustTrans, DocumentDate),
                date2StrXpp(_fromDate),
                date2StrXpp(_toDate),
                fieldStr(CustTrans, TransDate),
                date2StrXpp(dateNull())
            ));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(
            strFmt('((%1.%2 != "") || (%1.%2 == "") && (%1.%3 > 0))',
                custTransQueryBuildDataSource.name(),
                fieldStr(CustTrans, Invoice),
                fieldStr(CustTrans, AmountMST)
            ));

        return query;
    }`
    - **Method:**
      - **Name:** `openinvoiceBalanceMSTDoc`
      - **Source:** `AmountMST openinvoiceBalanceMSTDoc(TransDate   _transDate  = dateNull(),
                                       FromDate    _fromDate   = dateNull(),
                                       ToDate      _toDate     = dateMax())
    {
        CustTransOpen  custTransOpen;
        AmountMST      amountMST = 0;

        QueryRun queryRun = new QueryRun(this.openinvoiceBalanceMSTDocQuery(_transDate, _fromDate, _toDate));
        while (queryRun.next())
        {
            custTransOpen = queryRun.get(tableNum(CustTransOpen));

            amountMST += custTransOpen.AmountMST;
        }

        return amountMST;
    }`
    - **Method:**
      - **Name:** `openInvoiceBalanceMSTDueQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening invoice balance.
    /// </summary>
    /// <param name = "_transDate">
    /// Transaction date value.
    /// </param>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    public Query openInvoiceBalanceMSTDueQuery(
        TransDate   _transDate,
        FromDate    _fromDate,
        ToDate      _toDate)
    {
        Query query = new Query();

        QueryBuildDataSource custTransOpenQueryBuildDataSource = query.addDataSource(tableNum(CustTransOpen));

        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, AmountMST), SelectionField::Sum);
        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, RecId), SelectionField::Count);

        custTransOpenQueryBuildDataSource.addGroupByField(fieldNum(CustTransOpen, RefRecId));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, AccountNum)).value(queryValue(this.AccountNum));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, TransDate)).value(SysQuery::range(null, _transDate, true));
        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, DueDate)).value(SysQuery::range(_fromDate, _toDate, true));

        QueryBuildDataSource custTransQueryBuildDataSource = custTransOpenQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::InnerJoin);
        custTransQueryBuildDataSource.addSelectionField(fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addLink(fieldNum(CustTransOpen, RefRecId), fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(
                strFmt('((%1.%2 != "") || ((%1.%2 == "") && (%1.%3 > 0)))',
                custTransQueryBuildDataSource.name(),
                fieldStr(CustTrans, Invoice),
                fieldStr(CustTrans, AmountMST)
            ));

        return query;
    }`
    - **Method:**
      - **Name:** `openInvoiceBalanceMSTDue`
      - **Source:** `AmountMST openInvoiceBalanceMSTDue(TransDate   _transDate  = dateNull(),
                                FromDate    _fromDate   = dateNull(),
                                ToDate      _toDate     = dateMax())
    {
        CustTransOpen custTransOpen;
        AmountMST amountMST = 0;
        QueryRun queryRun = new QueryRun(this.openInvoiceBalanceMSTDueQuery(_transDate, _fromDate, _toDate));

        while (queryRun.next())
        {
            custTransOpen = QueryRun.get(tableNum(CustTransOpen));
            amountMST += custTransOpen.AmountMST;
        }

        return amountMST;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMSTAssessmentDateQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening payment balance.
    /// </summary>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>
    /// <param name = "_assessmentDate">
    /// Assessment date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openPaymentBalanceMSTAssessmentDateQuery(
        FromDate   _fromDate,
        ToDate     _toDate,
        TransDate  _assessmentDate)
    {
        Query query = new Query();

        QueryBuildDataSource custSettlementQueryBuildDataSource = query.addDataSource(tableNum(CustSettlement));

        custSettlementQueryBuildDataSource.addSelectionField(fieldNum(CustSettlement, SettleAmountMST), SelectionField::Sum);
        custSettlementQueryBuildDataSource.addSelectionField(fieldNum(CustSettlement, ExchAdjustment), SelectionField::Sum);

        custSettlementQueryBuildDataSource.addGroupByField(fieldNum(CustSettlement, AccountNum));

        custSettlementQueryBuildDataSource.addRange(fieldNum(CustSettlement, AccountNum)).value(queryValue(this.AccountNum));

        custSettlementQueryBuildDataSource.addRange(fieldNum(CustSettlement, TransDate)).value('>' + SysQuery::value(_assessmentDate));

        QueryBuildDataSource custTransQueryBuildDataSource = custSettlementQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::InnerJoin);

        custTransQueryBuildDataSource.addLink(fieldNum(CustSettlement, TransRecId), fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, TransDate)).value(SysQuery::range(_fromDate, _toDate, true));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(SysQuery::valueEmptyString());
        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, AmountMST)).value('<0');

        return query;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMSTQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening payment balance.
    /// </summary>
    /// <param name = "_transDate">
    /// Transaction date value.
    /// </param>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openPaymentBalanceMSTQuery(
        FromDate   _fromDate,
        ToDate     _toDate)
    {
        Query query = new Query();

        QueryBuildDataSource custTransOpenQueryBuildDataSource = query.addDataSource(tableNum(CustTransOpen));

        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, AmountMST), SelectionField::Sum);

        custTransOpenQueryBuildDataSource.addGroupByField(fieldNum(CustTransOpen, RefRecId));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, AccountNum)).value(queryValue(this.AccountNum));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, TransDate)).value(SysQuery::range(_fromDate, _toDate, true));

        QueryBuildDataSource custTransQueryBuildDataSource = custTransOpenQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::InnerJoin);
        custTransQueryBuildDataSource.addSelectionField(fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addLink(fieldNum(CustTransOpen, RefRecId), fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(SysQuery::valueEmptyString());
        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, AmountMST)).value('<0');

        return query;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMST`
      - **Source:** `display AmountMST openPaymentBalanceMST(FromDate   _fromDate       = dateNull(),
                                            ToDate     _toDate         = dateMax(),
                                            TransDate  _assessmentDate = dateNull())
    {
        CustTrans       custTrans;
        CustTransOpen   custTransOpen;
        CustSettlement  custSettlement;
        AmountMST       openBalanceMST = 0;

        if (_assessmentDate)
        {
            if (!(hasFieldAccess(tableNum(CustSettlement), fieldNum(CustSettlement, SettleAmountMST) &&
                  hasFieldAccess(tableNum(CustSettlement), fieldNum(CustSettlement, ExchAdjustment)))))
            {
                throw error("@SYS57330");
            }

            QueryRun queryRun = new QueryRun(this.openPaymentBalanceMSTAssessmentDateQuery(_fromDate, _toDate, _assessmentDate));

            if (queryRun.next())
            {
                custSettlement = queryRun.get(tableNum(CustSettlement));
                openBalanceMST  = custSettlement.SettleAmountMST + custSettlement.ExchAdjustment;
            }
            openBalanceMST += this.openPaymentBalanceMST(_fromDate, _toDate);
        }
        else
        {
            if (!hasFieldAccess(tableNum(CustTransOpen), fieldNum(CustTransOpen, AmountMST)))
            {
                throw error("@SYS57330");
            }

            Query query = this.openPaymentBalanceMSTQuery(_fromDate, _toDate);
            query.clearGroupBy();

            QueryRun queryRun = new QueryRun(query);

            while (queryRun.next())
            {
                custTransOpen = queryRun.get(tableNum(CustTransOpen));
                openBalanceMST += custTransOpen.AmountMST;
            }
        }

        return openBalanceMST;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMSTDocQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening payment balance.
    /// </summary>
    /// <param name = "_transDate">
    /// Transaction date value.
    /// </param>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openPaymentBalanceMSTDocQuery(
        TransDate   _transDate  = dateNull(),
        FromDate    _fromDate   = dateNull(),
        ToDate      _toDate     = dateMax())
    {
        Query query = new Query();

        QueryBuildDataSource custTransOpenQueryBuildDataSource = query.addDataSource(tableNum(CustTransOpen));

        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, AmountMST), SelectionField::Sum);

        custTransOpenQueryBuildDataSource.addOrderByField(fieldNum(CustTransOpen, RefRecId));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, AccountNum)).value(queryValue(this.AccountNum));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, TransDate)).value(SysQuery::range(null, _transDate, true));

        QueryBuildDataSource custTransQueryBuildDataSource = custTransOpenQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::ExistsJoin);

        custTransQueryBuildDataSource.addLink(fieldNum(CustTransOpen, RefRecId), fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, DocumentDate)).value(
            strFmt('(((%1.%2 != %6) && (%1.%2 >= %3) && (%1.%2 <= %4)) || ((%1.%2 == %6) && (%1.%5 >= %3) && (%1.%5 <= %4)))',
                custTransQueryBuildDataSource.name(),
                fieldStr(CustTrans, DocumentDate),
                date2StrXpp(_fromDate),
                date2StrXpp(_toDate),
                fieldStr(CustTrans, TransDate),
                date2StrXpp(dateNull())
            ));

        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(SysQuery::valueEmptyString());
        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, AmountMST)).value('<0');

        return query;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMSTDoc`
      - **Source:** `AmountMST openPaymentBalanceMSTDoc(TransDate   _transDate  = dateNull(),
                                       FromDate    _fromDate   = dateNull(),
                                       ToDate      _toDate     = dateMax())
    {
        CustTrans       custTrans;
        CustTransOpen   custTransOpen;
        AmountMST       amountMST;

        QueryRun        queryRun = new QueryRun(this.openPaymentBalanceMSTDocQuery(_transDate, _fromDate, _toDate));
        while (queryRun.next())
        {
            custTransOpen = queryRun.get(tableNum(CustTransOpen));
            amountMST += custTransOpen.AmountMST;
        }

        return amountMST;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMSTDueQuery`
      - **Source:** `/// <summary>
    /// Creates query object for opening payment balance.
    /// </summary>
    /// <param name = "_transDate">
    /// Transaction date value.
    /// </param>
    /// <param name = "_fromDate">
    /// From date value.
    /// </param>
    /// <param name = "_toDate">
    /// To date value.
    /// </param>
    /// <returns>
    /// A <c>Query</c> object.
    /// </returns>
    protected Query openPaymentBalanceMSTDueQuery(TransDate   _transDate,
                                                  FromDate    _fromDate,
                                                  ToDate      _toDate)
    {
        Query query = new Query();

        QueryBuildDataSource custTransOpenQueryBuildDataSource = query.addDataSource(tableNum(CustTransOpen));

        custTransOpenQueryBuildDataSource.addSelectionField(fieldNum(CustTransOpen, AmountMST), SelectionField::Sum);

        custTransOpenQueryBuildDataSource.addGroupByField(fieldNum(CustTransOpen, RefRecId));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, AccountNum)).value(queryValue(this.AccountNum));

        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, TransDate)).value(SysQuery::range(null, _transDate, true));
        custTransOpenQueryBuildDataSource.addRange(fieldNum(CustTransOpen, DueDate)).value(SysQuery::range(_fromDate, _toDate, true));

        QueryBuildDataSource custTransQueryBuildDataSource = custTransOpenQueryBuildDataSource.addDataSource(tableNum(CustTrans));

        custTransQueryBuildDataSource.joinMode(JoinMode::InnerJoin);
        custTransQueryBuildDataSource.addSelectionField(fieldNum(CustTrans, RecId));

        custTransQueryBuildDataSource.addLink(fieldNum(CustTransOpen, RefRecId), fieldNum(CustTrans, RecId));
        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, Invoice)).value(SysQuery::valueEmptyString());
        custTransQueryBuildDataSource.addRange(fieldNum(CustTrans, AmountMST)).value('<0');
        return query;
    }`
    - **Method:**
      - **Name:** `openPaymentBalanceMSTDue`
      - **Source:** `AmountMST openPaymentBalanceMSTDue(TransDate   _transDate  = dateNull(),
                                       FromDate    _fromDate   = dateNull(),
                                       ToDate      _toDate     = dateMax())
    {
        AmountMST       amountMST;
        CustTransOpen   custTransOpen;
        QueryRun        queryRun = new QueryRun(this.openPaymentBalanceMSTDueQuery(_transDate, _fromDate, _toDate));

        while (queryRun.next())
        {
            custTransOpen = queryRun.get(tableNum(CustTransOpen));

            amountMST += custTransOpen.AmountMST;
        }

        return amountMST;
    }`
    - **Method:**
      - **Name:** `partyINNasOfDate_RU`
      - **Source:** `/// <summary>
    /// Accessor method to party attributes with respect to date effectivity.
    /// </summary>
    /// <param name="_date">
    /// Date as of which the attribute is fetched.
    /// </param>
    /// <returns>
    /// Value of INN property for the current party and date.
    /// </returns>
    public INN_RU partyINNasOfDate_RU(TransDate _date = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        return TaxRegistration::legislationRegistrationValue(this.Party, TaxRegistrationTypesList::INN, _date);
    }`
    - **Method:**
      - **Name:** `partyKPPasOfDate_RU`
      - **Source:** `/// <summary>
    /// Accessor method to party attributes with respect to date effectivity.
    /// </summary>
    /// <param name="_date">
    /// Date as of which the attribute is fethed.
    /// </param>
    /// <returns>
    /// Value of KPP property for the current party and date.
    /// </returns>
    public KPPU_RU partyKPPasOfDate_RU(TransDate _date = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        return TaxRegistration::legislationRegistrationValue(this.Party, TaxRegistrationTypesList::KPP, _date);
    }`
    - **Method:**
      - **Name:** `partyOKATOasOfDate_RU`
      - **Source:** `/// <summary>
    /// Accessor method to party attributes with respect to date effectivity.
    /// </summary>
    /// <param name="_date">
    /// Date as of which the attribute is fethed.
    /// </param>
    /// <returns>
    /// Value of OKATO property for the current party and date.
    /// </returns>
    public OKATO_RU partyOKATOasOfDate_RU(TransDate _date = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        return TaxRegistration::legislationRegistrationValue(this.Party, TaxRegistrationTypesList::OKATO, _date);
    }`
    - **Method:**
      - **Name:** `partyOKDPasOfDate_RU`
      - **Source:** `/// <summary>
    /// Accessor method to party attributes with respect to date effectivity.
    /// </summary>
    /// <param name="_date">
    /// Date as of which the attribute is fethed.
    /// </param>
    /// <returns>
    /// Value of OKDP property for the current party and date.
    /// </returns>
    public OKDP_RU partyOKDPasOfDate_RU(TransDate _date = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        return TaxRegistration::legislationRegistrationValue(this.Party, TaxRegistrationTypesList::OKDP, _date);
    }`
    - **Method:**
      - **Name:** `partyOKPOasOfDate_RU`
      - **Source:** `/// <summary>
    /// Accessor method to party attributes with respect to date effectivity.
    /// </summary>
    /// <param name="_date">
    /// Date as of which the attribute is fethed.
    /// </param>
    /// <returns>
    /// Value of OKPO property for the current party and date.
    /// </returns>
    public OKPO_RU partyOKPOasOfDate_RU(TransDate _date = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        return TaxRegistration::legislationRegistrationValue(this.Party, TaxRegistrationTypesList::OKPO, _date);
    }`
    - **Method:**
      - **Name:** `partyType`
      - **Source:** `DirPartyType partyType()
    {
        return DirPartyTable::findRec(this.Party).type();
    }`
    - **Method:**
      - **Name:** `paymentType`
      - **Source:** `/// <summary>
    /// Gets the payment type of the current record in the <c>CustPaymModeTable</c> table.
    /// </summary>
    /// <returns>
    /// The payment type; otherwise, an empty string;
    /// </returns>
    display PaymType paymentType()
    {
        CustPaymModeTable custPaymMode;
        str paymType;

        select firstonly custPaymMode
            where this.PaymMode == custPaymMode.PaymMode;
        if (custPaymMode)
        {
            paymType = enum2str(custPaymMode.PaymentType);
        }

        return paymType;
    }`
    - **Method:**
      - **Name:** `paymName`
      - **Source:** `display Description paymName()
    {
        return this.CustVendTable::paymName();
    }`
    - **Method:**
      - **Name:** `phone`
      - **Source:** `display Phone phone()
    {
        LogisticsElectronicAddress electronicAddress;

        electronicAddress = DirParty::primaryElectronicAddress(this.Party, LogisticsElectronicAddressMethodType::Phone);

        if (electronicAddress)
        {
            Phone phone = electronicAddress.Locator;
            electronicAddress.dispose();
            return phone;
        }

        return '';
    }`
    - **Method:**
      - **Name:** `phoneLocal`
      - **Source:** `display PhoneLocal phoneLocal()
    {
        LogisticsElectronicAddress electronicAddress;

        electronicAddress = DirParty::primaryElectronicAddress(this.Party, LogisticsElectronicAddressMethodType::Phone);

        if (electronicAddress)
        {
            PhoneLocal phone = electronicAddress.LocatorExtension;
            electronicAddress.dispose();
            return phone;
        }

        return '';
    }`
    - **Method:**
      - **Name:** `postalAddress`
      - **Source:** `LogisticsPostalAddress postalAddress()
    {
        return DirParty::primaryPostalAddress(this.Party);
    }`
    - **Method:**
      - **Name:** `postalAddressRecId`
      - **Source:** `/// <summary>
    /// Retrieves the record ID of the primary postal address for the
    /// current <c>CustTable</c> record.
    /// </summary>
    /// <returns>
    /// The record ID of the primary postal address for the
    /// current <c>CustTable</c> record.
    /// </returns>
    LogisticsPostalAddressRecId postalAddressRecId()
    {
        return DirParty::primaryPostalAddress(this.Party).RecId;
    }`
    - **Method:**
      - **Name:** `previewPaneTitle`
      - **Source:** `/// <summary>
    /// Retrieves the title field that is on top of the preview pane.
    /// </summary>
    /// <returns>
    /// The title field of the preview pane.
    /// </returns>
    display Caption previewPaneTitle()
    {
        return strFmt("@SYS327590", this.AccountNum, this.name());
    }`
    - **Method:**
      - **Name:** `priceDiscGroupName`
      - **Source:** `display PriceDiscName priceDiscGroupName()
    {
        return PriceDiscGroup::find(ModuleInventCustVend::Cust,
                                    PriceGroupType::PriceGroup,
                                    this.PriceGroup).Name;
    }`
    - **Method:**
      - **Name:** `projectListName`
      - **Source:** `display Name projectListName()
    {
        return "@SYS62154";
    }`
    - **Method:**
      - **Name:** `promptCust`
      - **Source:** `str promptCust()
    {
        return strFmt('%1 %2\n%3 %4\n%5 %6',fieldPName(CustTable, AccountNum), this.AccountNum,
                                            fieldPName(DirPartyTable, Name), this.name(),
                                            fieldPName(LogisticsPostalAddress, Address), this.postalAddress().Address);
    }`
    - **Method:**
      - **Name:** `remainAmountCurPeriod`
      - **Source:** `display AmountCur remainAmountCurPeriod(TransDate    _fromDate     = dateNull(),
                                            TransDate    _todate       = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()),
                                            CurrencyCode _currencyCode = this.Currency)
    {
        CustTrans   custTrans;

        select sum (AmountCur), sum (SettleAmountCur) from custTrans
            group by CurrencyCode
            where custTrans.AccountNum      == this.AccountNum  &&
                  custTrans.TransDate       >= _fromDate        &&
                  custTrans.TransDate       <= _todate          &&
                  custTrans.CurrencyCode    == _currencyCode;

        return (custTrans.AmountCur - custTrans.SettleAmountCur);
    }`
    - **Method:**
      - **Name:** `renamePrimaryKey`
      - **Source:** `public void renamePrimaryKey()
    {
        if (isConfigurationkeyEnabled(configurationKeyNum(Retail)))
        {
            RetailConnActionManagement::errorOnRename(this);
        }

        // Here this.AccountNum refers the new account number to be renamed
        if (CustAccountRenameSameAsExistingAccountDataMaintenanceFlight::instance().isEnabled() && CustTable::exist(this.AccountNum))
        {
            throw Error(strfmt("@AccountsReceivable:CustAccountAlreadyExistError", this.AccountNum));
        }

        CustSharedTable custSharedTable;

        select firstonly custSharedTable;

        // Check if all the references are synced or not by the initial <C>CustAccountNumObjectReferencesSetup</C>
        if (custSharedTable && custSharedTable.FailedCustTableAccountNumReferenceCount != 0)
        {
            CustAccountNumObjectReferenceProcessor custAccountNumObjectReferenceProcessor = new CustAccountNumObjectReferenceProcessor();

            // Regenerate the references
            ttsbegin;
            custAccountNumObjectReferenceProcessor.getListOfTableAndFields(extendedTypeStr(CustAccount));
            ttscommit;
        }

        DimensionValueRenameV2 rename = DimensionValueRenameV2::construct(this, this.orig());
        rename.syncRenamedValuePreSuper();

        Utcdatetime renamedDateTime = DateTimeUtil::getSystemDateTime();
        CustTable oldCustTable = this.orig() as CustTable;

        try
        {
            super();
        }
        catch
        {
            warning('@CreditCollections:CustAccountRenameExceptionTriggeredWarning');
        }
        finally
        {
            CustTable localCustTable;

            select firstonly RecId from localCustTable
                where localCustTable.AccountNum == this.AccountNum;

            if (CustAccountRenameSameAsExistingAccountDataMaintenanceFlight::instance().isEnabled() && localCustTable.RecId == oldCustTable.RecId && this.AccountNum != oldCustTable.AccountNum)
            {
                // Decided against including this functionality under the 'CustAccountNumRenameDataMaintenanceFeature' flight. Since this function automatically detects and
                // resolves out-of-sync records, enabling the feature after the action is performed renders it ineffective. Hence, we're keeping this functionality outside of feature control
                // to ensure its availability regardless of the feature's status.
                CustAccountNumRenameDataMaintenanceService::generateCustAccountNumRenameDataMaintenance(oldCustTable.AccountNum, this.AccountNum, renamedDateTime);
            }
        }

        rename.syncRenamedValuePostSuper();
        MCRFullTextSearch::update(this);
    }`
    - **Method:**
      - **Name:** `setInventProfileId_RU`
      - **Source:** `/// <summary>
    /// Update Inventory profile based on Inventory profile type
    /// </summary>
    void setInventProfileId_RU()
    {
        if (this.InventProfileType_RU == InventProfileType_RU::NotSpecified ||
            this.InventProfileType_RU != InventProfile_RU::find(this.InventProfileId_RU).InventProfileType)
        {
            this.InventProfileId_RU = '';
        }
    }`
    - **Method:**
      - **Name:** `settlementBuffer`
      - **Source:** `CustSettlement settlementBuffer()
    {
        CustSettlement  custSettlement;

        return custSettlement;
    }`
    - **Method:**
      - **Name:** `shouldEstimateBeCalculated`
      - **Source:** `/// <summary>
    ///    Determines whether the estimate value should be calculated for the order.
    /// </summary>
    /// <returns>
    ///    true if the estimate value should be calculated for the order; otherwise, false.
    /// </returns>
    /// <remarks>
    ///    If the customer has a credit limit,
    ///    and the customer has specified mandatory credit limit
    ///    or the <c>typeOfCreditMaxCheck</c> property is set to <c>TypeOfCreditmaxCheck</c> enumeration value different from None
    ///    then credit limit check would use the estimate value and it needs to be calculated.
    /// </remarks>
    public boolean shouldEstimateBeCalculated()
    {
        boolean         shouldEstimateBeCalculated;

        if (this.CreditMax
            || (this.AccountNum != this.InvoiceAccount && CustTable::find(this.InvoiceAccount).CreditMax))
        {
            if (this.MandatoryCreditLimit)
            {
                shouldEstimateBeCalculated = true;
            }
            else
            {
                shouldEstimateBeCalculated = this.isEstimatedBasedOnCreditMaxCheck();
            }
        }

        return shouldEstimateBeCalculated;
    }`
    - **Method:**
      - **Name:** `isEstimatedBasedOnCreditMaxCheck`
      - **Source:** `/// <summary>
    /// Determines whether the estimate should be calculated based off of the <c>TypeOfCreditmaxCheck</c> setting for the customer.
    /// </summary>
    /// <returns>true if estimate should be calculated; otherwise, false.</returns>
    public boolean isEstimatedBasedOnCreditMaxCheck()
    {
        CustParameters custParameters;
        custParameters = CustParameters::find();

        return custParameters.CreditMaxCheck == TypeOfCreditmaxCheck::Balance
                    || custParameters.CreditMaxCheck == TypeOfCreditmaxCheck::BalanceAll
                    || custParameters.CreditMaxCheck == TypeOfCreditmaxCheck::BalanceDelivered;
    }`
    - **Method:**
      - **Name:** `showPartyMatchIcon`
      - **Source:** `display DirPartyMatchIcon showPartyMatchIcon()
    {
        return DirParty::showPartyMatchIcon(this);
    }`
    - **Method:**
      - **Name:** `statementAddress`
      - **Source:** `/// <summary>
    /// Find address used for statement.
    /// </summary>
    /// <returns>
    /// The <c>LogisticsPostalAddress</c> to send statements to.
    /// </returns>
    /// <remarks>
    /// Has fallback logic, so if a <c>LogisticsLocationRoleType::Statment</c> address is not found, it will return primary address instead.
    /// </remarks>
    LogisticsPostalAddress statementAddress()
    {
        LogisticsPostalAddress address = DirParty::postalAddress(this.Party, LogisticsLocationRoleType::Statement);

        if (!address)
        {
            address = DirParty::primaryPostalAddress(this.Party);
        }

        return address;
    }`
    - **Method:**
      - **Name:** `stateName`
      - **Source:** `display AddressStatename stateName()
    {
        LogisticsPostalAddress postalAddress = this.postalAddress();

        return LogisticsAddressState::find(postalAddress.CountryRegionId, postalAddress.State).Name;
    }`
    - **Method:**
      - **Name:** `streetName_BR`
      - **Source:** `/// <summary>
    ///     Gets the street name based on the customer primary postal address
    /// </summary>
    /// <returns>
    ///     The street name.
    /// </returns>
    display LogisticsAddressStreet streetName_BR()
    {
        return this.postalAddress().Street;
    }`
    - **Method:**
      - **Name:** `streetNumber_BR`
      - **Source:** `/// <summary>
    ///    Gets the StreetNumber of the customer.
    /// </summary>
    /// <returns>
    ///    Customer StreetNumber.
    /// </returns>
    display LogisticsAddressStreetNumber streetNumber_BR()
    {
        return this.postalAddress().StreetNumber;
    }`
    - **Method:**
      - **Name:** `subscriptionType_BR`
      - **Source:** `/// <summary>
    /// Gets national company/person number
    /// </summary>
    /// <returns>
    /// The national company or person number based on the type of party.
    /// </returns>
    display CNPJCPFNum_BR subscriptionType_BR()
    {
        if (this.partyType() == DirPartyType::Organization)
        {
            return '02';
        }
        else if (this.partyType() == DirPartyType::Person)
        {
            return '01';
        }
        else
        {
            return '';
        }
    }`
    - **Method:**
      - **Name:** `summaryLedgerDimension`
      - **Source:** `public LedgerDimensionDefaultAccount summaryLedgerDimension(CustPostingProfile _custPostingProfile = CustParameters::find().PostingProfile)
    {
        return CustLedgerAccounts::summaryLedgerDimension(this.AccountNum, _custPostingProfile);
    }`
    - **Method:**
      - **Name:** `taxGroupName`
      - **Source:** `display TaxGroupName taxGroupName()
    {
        return TaxGroupHeading::find(this.TaxGroup).TaxGroupName;
    }`
    - **Method:**
      - **Name:** `telefax`
      - **Source:** `display TeleFax telefax()
    {
        LogisticsElectronicAddress electronicAddress;

        electronicAddress = DirParty::primaryElectronicAddress(this.Party, LogisticsElectronicAddressMethodType::Fax);

        if (electronicAddress)
        {
            TeleFax fax = electronicAddress.Locator;
            electronicAddress.dispose();
            return fax;
        }

        return '';
    }`
    - **Method:**
      - **Name:** `transBuffer`
      - **Source:** `CustTrans transBuffer()
    {
        CustTrans   custTrans;

        return custTrans;
    }`
    - **Method:**
      - **Name:** `transOpenBuffer`
      - **Source:** `CustTransOpen transOpenBuffer()
    {
        CustTransOpen   custTransOpen;

        return custTransOpen;
    }`
    - **Method:**
      - **Name:** `update`
      - **Source:** `void update(boolean _updateSmmBusRelTable = true, boolean _updateParty = true)
    {
        CustTable   this_Orig = this.orig();
        RecVersion  rv = this_Orig.RecVersion;
        // <GEERU>
        #isoCountryRegionCodes
        // </GEERU>

        ttsbegin;

        // <GEERU>
        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU]))
        {
            this.setInventProfileId_RU();
        }
        // </GEERU>

        super();

        // Update the full text search table
        MCRFullTextSearch::update(this);

        this.SysExtensionSerializerMap::postUpdate();

        if (_updateSmmBusRelTable)
        {
            smmBusRelTable::updateFromCustTableSFA2(this, '', false);
        }

        if (this_Orig.CustGroup != this.CustGroup)
        {
            ForecastSales::setCustGroupId(this.AccountNum,
                                          this_Orig.CustGroup,
                                          this.CustGroup);
        }

        smmTransLog::initTrans(this, smmLogAction::update);

        // If the customer group has changed
        if (this.CustGroup != this_Orig.CustGroup)
        {
            // clear the ledger cache
            LedgerCache::clearScope(LedgerCacheScope::PartyMainAccountDimensionListProvCust);
        }

        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]) && _updateParty)
        {
            this.copyInfoToParty();
        }

        // <GBR>
        if (BrazilParameters::isEnabled())
        {
            if (!CustVendTableFiscalInformationCopyCheckFlight::instance().isEnabled() || !FiscalInformationCopy_BR::twoCustVendTableHaveSameFiscalInformation(this_Orig, this))
            {
                FiscalInformationCopy_BR::copyFiscalInfoToCustVend(this);
            }
        }
        // </GBR>

        ttscommit;
    }`
    - **Method:**
      - **Name:** `url`
      - **Source:** `display URL url()
    {
        LogisticsElectronicAddress electronicAddress;

        electronicAddress = DirParty::primaryElectronicAddress(this.Party, LogisticsElectronicAddressMethodType::URL);

        if (electronicAddress)
        {
            URL url = electronicAddress.Locator;
            electronicAddress.dispose();
            return url;
        }

        return '';
    }`
    - **Method:**
      - **Name:** `validateBlocked`
      - **Source:** `/// <summary>
    ///     Verifies that the selected value for <c>Blocked</c> is valid for a record in <c>CustTable</c>.
    /// </summary>
    /// <param name="_blocked">
    ///     The value being validated.
    /// </param>
    /// <returns>
    ///     True if the value is valid for a record in <c>CustTable</c>
    /// </returns>
    /// <remarks>
    ///     The enum <c>CustVendorBlocked</c> is used in multiple tables, requiring
    ///     validation to eliminate selecting values that do not apply for <c>CustTable</c>.
    /// </remarks>
    protected boolean validateBlocked(CustBlocked _blocked)
    {
        boolean ret = true;

        switch (_blocked)
        {
            case CustVendorBlocked::Never:
            case CustVendorBlocked::Payment:
            case CustVendorBlocked::Requisition:
            case CustVendorBlocked::PurchOrder:
                ret = checkFailed(strFmt("@SYS137754", "@SYS316613"));
                break;

            default:
                break;
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `validateDelete`
      - **Source:** `boolean validateDelete()
    {
        boolean         ret;
        CustTable       custTable;
        SalesTable      salesTable;

        ret = super();

        if (this.AccountNum)
        {
            select firstonly RecId from custTable
                where custTable.InvoiceAccount == this.AccountNum;

            if (custTable.RecId)
            {
                ret = checkFailed("@SYS67133");
            }

            select firstonly RecId from salesTable
                where salesTable.InvoiceAccount == this.AccountNum;

            if (salesTable.RecId)
            {
                ret = checkFailed(strFmt("@SYS75284", tablePName(SalesTable)));
            }
        }

        if (ret)
        {
            ret = CustVendNetAgreementRelationship::validatePartyAssociationNotInCustVendNetAgreementRelationship(this);
        }

        if (ret && this.CustVendTable::isRelatedToIncompleteWorkflowWorkItems())
        {
            ret = checkFailed("@AccountsReceivable:CustomerDeletionWorkflowWorkItemError");
        }

        if (ret && this.isActiveOnCustHierarchyNode())
        {
            ret = checkFailed("@CustHierarchy:CustDeleteErrorMessage");
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `isActiveOnCustHierarchyNode`
      - **Source:** `/// <summary>
    /// Checks whether a customer record is associated to an active customer hierarchy node.
    /// </summary>
    /// <returns>True in case the customer is on a hierarchy; false, otherwise.</returns>
    private boolean isActiveOnCustHierarchyNode()
    {
        CustHierarchyNode custHierarchyNode;

        select firstonly RecId from custHierarchyNode
            where   custHierarchyNode.Party == this.Party
                &&  (!custHierarchyNode.VersionRemoved || custHierarchyNode.VersionAdded > custHierarchyNode.VersionRemoved);

        return custHierarchyNode.RecId != 0;
    }`
    - **Method:**
      - **Name:** `validateField`
      - **Source:** `boolean validateField(FieldId p1)
    {
        boolean                     ret;

        TaxRegistrationValidator_MX taxRegistrationValidator;
        #isoCountryRegionCodes

        int                         lengthAgencyLocationCode;

        // <GJP>
        #define.MinConsDayZero(0)
        #define.MaxConsDayThirtyOne(31)
        // </GJP>
        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
        {
            taxRegistrationValidator = TaxRegistrationValidator_MX::construct(this);
        }

        ret = super(p1);

        if (ret)
        {
            switch (p1)
            {
                case fieldNum(CustTable, vatNum) :
                    ret = TaxVATNumTable::checkVATNum(this.vatNum, this, p1);
                    if (this.vatNum &&
                        LogisticsAddressCountryRegion::find(this.postalAddress().CountryRegionId).isOcode == #isoBE &&
                        SysCountryRegionCode::isLegalEntityInCountryRegion([#isoBE]))
                    {
                        ret = ret && TaxEnterpriseBranchNumber_BE::checkEnterPriseNumber(this.getPrimaryRegistrationNumber(TaxRegistrationTypesList::UID), this.vatNum, true);
                    }
                    if (ret)
                    {
                        this.checkVATNumUsed();
                    }
                    break;

                case fieldNum(CustTable, CreditMax) :
                    if (this.CreditMax < 0)
                    {
                        ret = checkFailed("@SYS69970");
                        // <GEERU>
                    }
                    else if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU])
                        && CustParameters::find().AgreementCreditLine_RU)
                    {
                        ret = AgreementHeaderExt_RU::checkCreditLimitWithoutAgreement(ModuleSalesPurch::Sales, this.AccountNum, this.CreditMax);
                        // </GEERU>
                    }
                    break;

                case fieldNum(CustTable, InventLocation) :
                    ret = this.InventStorageDimMap::validateField(fieldNum(InventStorageDimMap, InventLocationId));
                    break;

                case fieldNum(CustTable, OrgId) :
                    GlobalizationInstrumentationHelper::featureRunByCountryRegionCodes(
                        [ [#isoIS, GlobalizationConstants::FeatureReferenceIS00002] ],
                        funcName()
                    );
                    if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoIS])
                        && CustVendSSNValidation_IS::checkSSN(this.OrgId) == false)
                    {
                        ret = checkFailed("@SYS113286");
                    }
                    break;

                // <GJP>
                case fieldNum(CustTable, ConsDay_JP) :
                    if (CustConsInvoiceType_JP::isCustConsInvoiceEnabled()
                        && !this.CustVendTable::isConsDayValid_JP())
                    {
                        // The value is out of its valid range - it must be between %1 and %2
                        ret = checkFailed(strFmt("@SYS87701", #MinConsDayZero, #MaxConsDayThirtyOne));
                    }
                    break;
                // </GJP>
                case fieldNum(CustTable, Rfc_MX) :
                    if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
                    {
                        ret = taxRegistrationValidator.validateRFC(this.Rfc_MX, this.CompanyType_MX);
                    }
                    break;

                case fieldNum(CustTable, Curp_MX) :
                    if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
                    {
                        ret = taxRegistrationValidator.validateCurp(this.Curp_MX, this.CompanyType_MX);
                    }
                    break;

                case fieldNum(CustTable, StateInscription_MX):
                    if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
                    {
                        ret = taxRegistrationValidator.validateStateInscription(this.StateInscription_MX);
                    }
                    break;

                case fieldNum(CustTable, MainContactWorker) :
                    if (this.MainContactWorker && !(CustTableAllowCrossCompanyEmployeeFlight::instance().isEnabled() && CustParameters::find().AllowCrossCompanyEmployee))
                    {
                        ret = smmUtility::isValidWorkerInCurrentCompany(this.MainContactWorker);
                    }
                    break;

                case fieldNum(CustTable, Blocked) :
                    ret = this.validateBlocked(this.Blocked);
                    break;

                case fieldNum(CustTable, PrepaymentValue) :
                    ret = this.isValidPrepaymentAmount(this.PrepayType);
                    break;

                case fieldNum(CustTable, EinvoiceEANNum):
                    if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoDK]))
                    {
                        ret = CustTable::checkEInvoiceEAN(this.EinvoiceEANNum);
                    }
                    break;

                case fieldNum(CustTable, CashDiscBaseDays):
                    if (this.CashDiscBaseDays < 0)
                    {
                        this.CashDiscBaseDays = 0;
                        ret = checkFailed("@SYS118311");
                    }
                    break;

                case fieldNum(CustTable, AgencyLocationCode) :
                    if (isConfigurationkeyEnabled(configurationKeyNum(PublicSector)))
                    {
                        if (this.AgencyLocationCode != '')
                        {
                            lengthAgencyLocationCode = strLen(this.AgencyLocationCode);
                            if (lengthAgencyLocationCode < 11
                                || lengthAgencyLocationCode > 12)
                            {
                                ret = checkFailed("@SPS2440");
                            }
                        }
                    }
                    break;

                case fieldNum(CustTable, DefaultInventStatusId):
                    if (this.DefaultInventStatusId && WHSInventStatus::isBlockingStatus(this.DefaultInventStatusId))
                    {
                        ret = checkFailed("@WAX3363");
                    }
                    break;

                case fieldNum(CustTable, AuthorityOffice_IT) :
                    if (this.AuthorityOffice_IT && strLen(strLRTrim(this.AuthorityOffice_IT)) < 6)
                    {
                        ret = checkFailed("@ApplicationSuite_InvoicesCommunication:EnterValidAuthorityOfficeCode");
                    }
                    break;
            }

            // <GBR>
            if (BrazilParameters::isEnabled())
            {
                ret = this.validateField_BR(p1);
            }
            // </GBR>
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `validateField_BR`
      - **Source:** `private boolean validateField_BR(fieldId _fieldId)
    {
        boolean ret = true;

        switch (_fieldId)
        {
            case fieldnum(CustTable, CNPJCPFNum_BR):
                ret = this.validateCNPJCPF_BR();
                break;

            case fieldnum(CustTable, CNAE_BR):
                if (this.CNAE_BR)
                {
                    ret = FiscalInformationUtil_BR::isCNAEValid(this.CNAE_BR);

                    if (ret)
                    {
                        this.CNAE_BR = FiscalInformationUtil_BR::formatCNAE(this.CNAE_BR);
                    }
                }
                break;

            case fieldnum(CustTable, IENum_BR):
                if (this.postalAddress().State && this.IENum_BR)
                {
                    ret = FiscalInformationUtil_BR::validateIENum(this.partyType(), this.IENum_BR, this.AccountNum, this.postalAddress().State);
                }
                break;

            case fieldNum(CustTable, SuframaNumber_BR):
                if (this.SuframaNumber_BR)
                {
                    if (this.SuframaNumber_BR && strLen(this.SuframaNumber_BR) != 9)
                    {
                        ret = checkFailed(strFmt("@GLS64298", fieldPName(CustTable, SuframaNumber_BR), 9));
                    }
                }
                break;

            case fieldNum(CustTable, ForeignerId_BR):
                if (this.ForeignerId_BR)
                {
                    ret = FiscalInformationUtil_BR::isForeignerIdValid(this.ForeignerId_BR);
                }
                break;
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `validateCNPJCPF_BR`
      - **Source:** `/// <summary>
    ///     Validate if CNPJCFPF information is correctly formatted.
    /// </summary>
    /// <returns>
    ///     true if the record is valid; otherwise, false.
    /// </returns>
    protected boolean validateCNPJCPF_BR()
    {
        DirPartyType partyType = DirPartyTable::findRec(this.Party).type();

        boolean ret = FiscalInformationUtil_BR::validateCNPJCPFNumByType(partyType, this.CNPJCPFNum_BR);

        if (ret)
        {
            CNPJNum_BR cnpjNum_BR =  FiscalInformationUtil_BR::formatCNPJCPF(partyType, this.CNPJCPFNum_BR);

            CustTable       custTableLoc;

            select firstonly custTableLoc
                    where custTableLoc.CNPJCPFNum_BR != '' &&
                          custTableLoc.CNPJCPFNum_BR  == cnpjNum_BR &&
                          custTableLoc.AccountNum != this.AccountNum;

            if (custTableLoc)
            {
                ret = checkFailed(strfmt("@GLS40", custTableLoc.AccountNum, custTableLoc.name()));
            }
            else
            {
                this.CNPJCPFNum_BR = cnpjNum_BR;
            }
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `validateWrite`
      - **Source:** `/// <summary>
    ///    Determines whether the current record is valid and ready to be written to the database.
    /// </summary>
    /// <returns>
    ///    true if the record is valid; otherwise, false.
    /// </returns>
    boolean validateWrite()
    {
        #isoCountryRegionCodes
        boolean                     ret;
        DirPartyType                type;

        TaxRegistrationValidator_MX taxRegistrationValidator;
        // <GEERU>
        CustTable                   this_orig = this.orig();
        // </GEERU>

        ret = super();

        // Warn user if this customer lacks a tax exempt number, but do not fail to save the revision.
        if (!this.OneTimeCustomer && TaxVATNumTable::isVATNumMandatory(CustParameters::find().MandatoryVATNum, this))
        {
            warning("@SYS54494");
        }

        if (ret)
        {
            if (PaymTerm::isCashAccount(this.PaymTermId) && this.PaymSched)
            {
                ret = checkFailed("@SYS25074");
            }

            if (TaxWithholdingGlobalFeature::isItemWHTSupportedInCountryRegionOrParamEnabled()
                && this.TaxWithholdCalculate_TH
                && this.TaxWithholdGroup_TH == '')
            {
                ret = checkFailed(strFmt("@SYS81766",
                                         fieldId2pname(tableNum(CustTable), fieldNum(CustTable, TaxWithholdCalculate_TH)),
                                         fieldId2pname(tableNum(CustTable), fieldNum(CustTable, TaxWithholdGroup_TH))));
            }

            ret = ret && this.isValidPrepaymentAmount(this.PrepayType);
        }

        // Add check MandatoryTaxGroup is set on CustParameters.
        if (ret && CustParameters::find().MandatoryTaxGroup && !this.TaxGroup)
        {
            ret = checkFailed("@SYS113299");
        }

        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoMX]))
        {
            taxRegistrationValidator = TaxRegistrationValidator_MX::construct(this);
            ret = taxRegistrationValidator.validateCustomerTaxRegistration() && ret;
        }

        if (this.MainContactWorker && !(CustTableAllowCrossCompanyEmployeeFlight::instance().isEnabled() && CustParameters::find().AllowCrossCompanyEmployee))
        {
            ret = ret && smmUtility::isValidWorkerInCurrentCompany(this.MainContactWorker);
        }

        // <GEERU>
        if (   ret
            && SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU])
            && this_orig.VendAccount
            && this_orig.VendAccount != this.VendAccount)
        {
            if (! this.checkRelatedAgreement_RU())
            {
                ret = checkFailed("@GLS115694");
            }
        }
        // </GEERU>

        // <GBR>
        if (BrazilParameters::isEnabled())
        {
            if (this.Suframa_BR && !this.SuframaNumber_BR)
            {
                ret = ret && checkFailed(strfmt("@GLS140", this.name()));
            }

            if (!this.Suframa_BR && (this.SuframaNumber_BR || this.SuframaPISCOFINS_BR))
            {
                ret = ret && checkFailed("@Brazil:CustTableNotSuframaRegionSuframaFieldsInformed");
            }

            if ((this.CustFinalUser_BR || this.isForeigner_BR()) && this.Suframa_BR)
            {
                ret = ret && checkFailed("@Brazil:CustTableFinalUserNotSuframa");
            }

            DirPartyType partyType = this.partyType();
            if (partyType == DirPartyType::Organization || partyType == DirPartyType::OperatingUnit || partyType == DirPartyType::LegalEntity)
            {
                if (this.NIT_BR)
                {
                    ret = ret && checkFailed(strfmt("@Brazil:CustTableFieldNotNeededByPartyType", enum2Str(partyType), fieldId2pname(tableNum(CustTable), fieldNum(CustTable, NIT_BR))));
                }
                if (this.INSSCEI_BR)
                {
                    ret = ret && checkFailed(strfmt("@Brazil:CustTableFieldNotNeededByPartyType", enum2Str(partyType), fieldId2pname(tableNum(CustTable), fieldNum(CustTable, INSSCEI_BR))));
                }
            }
        }
        // </GBR>

        if (ret && !CustVendTable::validateContactForParty(true, this.ContactPersonId, this.Party))
        {
            ret = checkFailed("@SCM:InvalidPrimaryContactErrorMessage");
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `isValidPrepaymentAmount`
      - **Source:** `private boolean isValidPrepaymentAmount(SalesPrepayType _salesPrepayType)
    {
        boolean isValid = true;

        switch (_salesPrepayType)
        {
            case SalesPrepayType::Percent:
                // Prepayment percent value must be between 0 and 100
                if(this.PrepaymentValue < 0 || this.PrepaymentValue > 100)
                {
                    isValid = checkFailed("@SYS183656");
                }
                break;

            case SalesPrepayType::Fixed:
                // Prepayment fixed value must be positive
                if (this.PrepaymentValue < 0)
                {
                    isValid = checkFailed("@SYS183657");
                }
                break;
        }

        return isValid;
    }`
    - **Method:**
      - **Name:** `vendorAccountName`
      - **Source:** `display VendInvoiceAccountName vendorAccountName()
    {
        VendTable       vend;
        DirPartyTable   party;

        if (this.VendAccount)
        {
            select RecId from vend
                join Name from party
                    where vend.AccountNum == this.VendAccount && party.RecId == vend.Party;
        }
        else
        {
            select RecId from vend
                join Name from party
                    where vend.Party == this.Party;
        }

        return party.Name;
    }`
    - **Method:**
      - **Name:** `zipCode_BR`
      - **Source:** `/// <summary>
    ///     Gets the zipcode based on the customer primary postal address
    /// </summary>
    /// <returns>
    ///     The zipcode.
    /// </returns>
    display LogisticsAddressZipCodeId zipCode_BR()
    {
        return this.postalAddress().ZipCode;
    }`
    - **Method:**
      - **Name:** `blocked`
      - **Source:** `static CustVendorBlocked blocked(CustAccount _custAccount)
    {
        return CustTable::find(_custAccount).Blocked;
    }`
    - **Method:**
      - **Name:** `canCustomerBeUpdated`
      - **Source:** `static boolean canCustomerBeUpdated(CustAccount         _custAccount,
                                        CustInvoiceAccount  _invoiceAccount,
                                        DocumentStatus      _documentStatus = DocumentStatus::None)
    {
        boolean  ok = true;

        if (_documentStatus == DocumentStatus::Invoice      ||
            _documentStatus == DocumentStatus::PickingList  ||
            _documentStatus == DocumentStatus::PackingSlip  ||
            _documentStatus == DocumentStatus::ProjectPackingSlip  ||
            // <GEERU>
            _documentStatus == DocumentStatus::FreeTextInvoice ||
            _documentStatus == DocumentStatus::Facture_RU)
            // </GEERU>
        {
            ok = (!(CustTable::blocked(_custAccount)    != CustVendorBlocked::No    ||
                    CustTable::blocked(_invoiceAccount) != CustVendorBlocked::No));
        }
        else
        {
            ok = (!(CustTable::blocked(_custAccount) == CustVendorBlocked::All));
        }

        return ok;
    }`
    - **Method:**
      - **Name:** `checkCreditLimit`
      - **Source:** `static boolean checkCreditLimit(CustAccount          _custAccount,
                                           TypeOfCreditmaxCheck _check          = CustParameters::find().CreditMaxCheck,
                                           AmountMST            _amountMST      = 0,
                                           boolean              _warning          = CustParameters::find().CreditLineError == CreditLineErrorType::Warning,
                                           AgreementHeaderExtRecId_RU _agreementHeaderExtRecId = 0,
                                           TableId                    _tableId                 = 0,
                                           RecId                      _recId                   = 0
                                           )
    {
        //<GEERU>
        #ISOCountryRegionCodes
        //</GEERU>

        CustCreditLimit     custCL = new CustCreditLimit(CustTable::find(_custAccount));

        custCL.addAmountMST(_amountMST);
        custCL.typeOfCreditMaxCheck(_check);
        custCL.warning(_warning);

        // <GEERU>
        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoRU]))
        {
            custCL.agreementCheckInit_RU(_agreementHeaderExtRecId, _tableId, _recId);
        }
        // </GEERU>

        return (custCL.check() || _warning); // return true if warning is true regardless of the check
    }`
    - **Method:**
      - **Name:** `checkEInvoiceEAN`
      - **Source:** `public static boolean checkEInvoiceEAN(EinvoiceEANNum _einvoiceEANNum)
    {
        boolean         ret          = true;
        EinvoiceEANNum  eanNum      = _einvoiceEANNum;

        int             eanSum;
        Counter         c;
        int             multiply    = 3;
        char            chkDgt;
        #isoCountryRegionCodes

        if (eanNum)
        {
            if (strLen(eanNum) != 13)
            {
                ret = checkFailed(strFmt("@SYS100770", eanNum));
            }

            if (ret && !str2IntOk(eanNum))
            {
                ret = checkFailed(strFmt("@SYS100771", eanNum));
            }

            for (c = 12; c >= 1; c--)
            {
                eanSum += multiply * str2int(subStr(eanNum, c, 1));

                if (multiply == 3)
                {
                    multiply = 1;
                }
                else
                {
                    multiply = 3;
                }
            }

            chkDgt = int2str(any2int(roundUp(eanSum, 10)) - eanSum);

            if (ret && subStr(eanNum, 13,1) != chkDgt)
            {
                ret = checkFailed(strFmt("@SYS100772", eanNum));
            }
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `checkExist`
      - **Source:** `static boolean checkExist(CustAccount _custAccount)
    {
        boolean ret = true;

        if (_custAccount && !CustTable::exist(_custAccount))
        {
            ret = checkFailed(strFmt(CustTable::txtNotExist(), _custAccount));
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `checkExistAndOpen`
      - **Source:** `static boolean checkExistAndOpen(CustAccount    _custAccount,
                                     AmountCur      _amountCur)
    {
        boolean     ret;
        CustTable   custTable;

        custTable = CustTable::find(_custAccount);

        if (! custTable)
        {
            ret = checkFailed(strFmt("@SYS4730", _custAccount));
        }
        else
        {
            ret = custTable.checkAccountBlocked(_amountCur);
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `confirmAndSaveCustGroupChange`
      - **Source:** `/// <summary>
    ///     Shows a confirmation dialog box if the <c>CustGroup</c> field value is modified and reverts to previous value if user selects cancel.
    /// </summary>
    /// <param name="_custTable">
    ///     The <c>CustTable</c> buffer modified
    /// </param>
    /// <param name="isCustGroupSetOnce">
    ///     The value will be true, when a new record was created and user has already set the <c>CustGroup</c> value on the form once.
    /// </param>
    /// <returns>
    ///     True, if value was modified. False, if the value was reverted to the original value.
    /// </returns>
    [Replaceable]
    public static boolean confirmAndSaveCustGroupChange(CustTable _custTable, boolean isCustGroupSetOnce)
    {
        DialogButton    buttonClicked;
        boolean         canceled;
        boolean         valueModified;

        if (_custTable.orig().CustGroup || isCustGroupSetOnce)
        {
            buttonClicked = Box::okCancel( "@SPS2398", DialogButton::Ok );
            canceled = (buttonClicked==DialogButton::Cancel);
        }

        if (canceled)
        {
            _custTable.CustGroup = _custTable.orig().CustGroup;
            valueModified = false;
        }
        else
        {
            _custTable.DefaultDimension = CustGroup::find(_custTable.CustGroup).DefaultDimension;
            valueModified = true;
        }

        return valueModified;
    }`
    - **Method:**
      - **Name:** `createOneTimeAccount`
      - **Source:** `/// <summary>
    /// Creates a one-time customer
    /// </summary>
    /// <param name="_common">
    /// A record from which to initialize customer.
    /// </param>
    /// <returns>
    /// The <c>CustAccount</c> object of the new customer.
    /// </returns>
    static CustAccount createOneTimeAccount(Common _common)
    {
        // Can one time customer be created?
        if (!CustTable::createOneTimeAccountValidate())
        {
            return '';
        }

        ttsbegin;

        // Create one time account num
        NumberSeq numberSeq = NumberSeq::newGetNum(CustParameters::numRefOneTimeCustomerAccount());

        NumberSequenceTable numberSequenceTable = CustParameters::numRefOneTimeCustomerAccount().numberSequenceTable();

        if (numberSequenceTable.Manual == NoYes::Yes)
        {
            throw Error("@AccountsReceivable:ValidateNumberSequenceOfOneTimeCustomer");
        }

        // Load default customer, set new party, accountnum
        CustTable custTable = CustTable::find(CustParameters::find().DefaultCust);
        custTable.AccountNum = numberSeq.num();
        custTable.Party = 0;
 
        CustomerEntity customerEntity = DirParty::constructFromCommon(custTable,
                                                       DateTimeUtil::getSystemDateTime(),
                                                       DirAppParameters::defaultPartyType(tableNum(CustTable)) == DirPartyBaseType::Person ? DirPartyType::Person : DirPartyType::Organization,
                                                       true,
                                                       false);

        CustTable::initializeOneTimeAccount(_common, customerEntity, custTable);

        customerEntity.insert();

        ttscommit;

        return custTable.AccountNum;
    }`
    - **Method:**
      - **Name:** `initializeOneTimeAccount`
      - **Source:** `/// <summary>
    /// Initializes a new one time customer account.
    /// </summary>
    /// <param name = "_common">A record from which to initialize customer.</param>
    /// <param name = "_customerEntity">The <c>CustomerEntity</c> instance to initialize.</param>
    /// <param name = "_custTable">The default <c>CustTable</c> record.</param>
    protected static void initializeOneTimeAccount(Common _common, CustomerEntity  _customerEntity, CustTable _custTable)
    {
        //<GEERU>
        #ISOCountryRegionCodes
        //</GEERU>

        switch (_common.TableId)
        {
            case tableNum(CustInvoiceTable) :
                _customerEntity.initFromCustInvoiceTable(_common as CustInvoiceTable);
                break;

            case tableNum(SalesTable) :
                _customerEntity.initFromSalesTable(_common as SalesTable);
                break;

            case tableNum(SalesQuotationTable) :
                _customerEntity.initFromSalesQuotationTable(_common as SalesQuotationTable);
                break;

            // <GEEU>
            case tableNum(CzCustAdvanceInvoiceTable) :
                _customerEntity.initFromCustAdvanceInvoiceTable_CZ(_common as CzCustAdvanceInvoiceTable);
                break;

            // </GEEU>

            case tableNum(CustPrepaymentInvoiceTable) :
                _customerEntity.initFromCustPrepaymentInvoiceTable(_common as CustPrepaymentInvoiceTable);
                break;

            default :
                // <GEERU>
                if (_common.TableId == tableNum(CustTable) &&
                    SysCountryRegionCode::isLegalEntityInCountryRegion([ #isoRU
                    // <GEEU>
                    , #isoPL, #isoCZ, #isoHU, #isoLT, #isoLV, #isoEE
                    // </GEEU>
                    ]))
                {
                    _customerEntity.parmName(DirPartyTable::getName(_common.(fieldNum(CustTable, Party))));
                    _custTable.OneTimeCustomer = true;
                }
                else
                {
                    // </GEERU>
                    throw error(strFmt("@SYS18917", tableId2name(_common.TableId)));
                    // <GEERU>
                }
            // </GEERU>
        }
    }`
    - **Method:**
      - **Name:** `createOneTimeAccountValidate`
      - **Source:** `static boolean createOneTimeAccountValidate()
    {
        boolean ok = true;

        if (!CustTable::find(CustParameters::find().DefaultCust))
        {
            ok = checkFailed("@SYS57574");
        }

        return ok;
    }`
    - **Method:**
      - **Name:** `creditMax`
      - **Source:** `static AmountMST creditMax(CustAccount _custAccount)
    {
        return CustTable::find(_custAccount).CreditMax;
    }`
    - **Method:**
      - **Name:** `deleteOneTimeAccount`
      - **Source:** `static void deleteOneTimeAccount(CustAccount _custAccount)
    {
        CustTable               custTable = CustTable::find(_custAccount);
        NumberSequenceTable     numberSequenceTable;
        NumberSequenceReference numberSequenceReference;
        SalesTable              salesTable;

        if (!custTable)
        {
            return;
        }

        select firstonly RecId from salesTable
            where salesTable.CustAccount == _custAccount;

        if (salesTable.RecId)
        {
            return;
        }

        ttsbegin;

        delete_from custTable
            where custTable.AccountNum == _custAccount;

        numberSequenceReference = CustParameters::numRefOneTimeCustomerAccount();
        if (numberSequenceReference)
        {
            numberSequenceTable = NumberSequenceTable::find(numberSequenceReference.NumberSequenceId);
            if (numberSequenceTable.Continuous)
            {
                NumberSeq::release(numberSequenceTable.NumberSequence, _custAccount);
            }
        }

        ttscommit;
    }`
    - **Method:**
      - **Name:** `exist`
      - **Source:** `static boolean exist(CustAccount _custAccount)
    {
        return _custAccount && (select firstonly RecId from custTable
                                    index hint AccountIdx
                                    where custTable.AccountNum == _custAccount).RecId != 0;
    }`
    - **Method:**
      - **Name:** `existDlvMode`
      - **Source:** `/// <summary>
    ///    Checks whether the delivery mode is used on a customer record.
    /// </summary>
    /// <param name="_dlvModeId">
    ///    The <c>DlvModeId</c> value that specifies the delivery mode record.
    /// </param>
    /// <returns>
    ///    true if the deliver mode is used on a customer; otherwise, false.
    /// </returns>
    public static boolean existDlvMode(DlvModeId _dlvModeId)
    {
        CustTable       custTable;

        if (_dlvModeId == '')
        {
            return false;
        }

        select firstonly
            RecId from custTable
        where
            custTable.DlvMode == _dlvModeId;

        return (custTable.RecId != 0);
    }`
    - **Method:**
      - **Name:** `existTransactions`
      - **Source:** `/// <summary>
    ///    Checks whether the transactions exist for the specified customer account.
    /// </summary>
    /// <param name="_custAccount">
    ///    The <c>_custAccount</c> value that specifies the customer account.
    /// </param>
    /// <returns>
    ///    true if the transactions exist for that customer Account; otherwise, false.
    /// </returns>
    static boolean existTransactions(CustAccount _custAccount)
    {
        CustTrans custTrans;

        select firstonly RecId from custTrans
            where custTrans.AccountNum == _custAccount;

        return (custTrans.RecId != 0);
    }`
    - **Method:**
      - **Name:** `existTransactionsInMyLegalEntities`
      - **Source:** `/// <summary>
    ///    Checks whether the transactions exist for the specified customer account party number with companies that the current user has access to.
    /// </summary>
    /// <param name="_party">
    ///    The party record ID of the customer record.
    /// </param>
    /// <returns>
    ///    true if the transactions exist for that customer Account party number; otherwise, false.
    /// </returns>
    internal static boolean existTransactionsInMyLegalEntities(DirPartyRecId _party)
    {
        CustTrans custTrans;
        CustTable custTable;
        MyLegalEntities myLegalEntities;

        select firstonly crosscompany RecId from custTrans
            exists join custTable
                where custTrans.AccountNum == custTable.AccountNum
                && custTable.Party == _party
            exists join myLegalEntities
                where myLegalEntities.DataArea == custTable.DataAreaId;

        return (custTrans.RecId != 0);
    }`
    - **Method:**
      - **Name:** `find`
      - **Source:** `static CustTable find(CustAccount   _custAccount,
                          boolean       _forUpdate = false)
    {
        CustTable custTable;

        if (_custAccount)
        {
            if (_forUpdate)
            {
                custTable.selectForUpdate(_forUpdate);
            }

            select firstonly custTable
                index hint AccountIdx
                where custTable.AccountNum == _custAccount;
        }

        return custTable;
    }`
    - **Method:**
      - **Name:** `findByCompany`
      - **Source:** `/// <summary>
    ///    Finds the specified record in the <c>CustTable</c> table.
    /// </summary>
    /// <param name="_company">
    ///    The company for the customer record.
    /// </param>
    /// <param name="_custAccount">
    ///    The account number for the customer record.
    /// </param>
    /// <param name="_forUpdate">
    ///    A Boolean value that indicates whether to read the record for update; optional.
    /// </param>
    /// <returns>
    ///    A record in the <c>CustTable</c> table; otherwise, an empty record.
    /// </returns>
    public static CustTable findByCompany(
        DataAreaId _company,
        CustAccount _custAccount,
        boolean _forUpdate = false)
    {
        CustTable cust;

        if (_company)
        {
            changecompany(_company)
            {
                cust = CustTable::find(_custAccount, _forUpdate);
            }
        }

        return cust;
    }`
    - **Method:**
      - **Name:** `findByKanaName_JP`
      - **Source:** `/// <summary>
    ///     Finds custTable record according to the kana name.
    /// </summary>
    /// <param name="_custNameKana">
    ///     Customer kana name
    /// </param>
    /// <param name="_forUpdate">
    ///     Flag for update
    /// </param>
    /// <returns>
    ///     CustTable record
    /// </returns>
    public static CustTable findByKanaName_JP(
        DirPartyName    _custNameKana,
        boolean         _forUpdate = false)
    {
        CustTable       custTable;
        DirOrganization dirOrganization;

        if (_custNameKana)
        {
            if (_forUpdate)
            {
                custTable.selectForUpdate(_forUpdate);
            }

            select custTable
                join RecId, PhoneticName from dirOrganization
                where dirOrganization.RecId == custTable.Party
                && dirOrganization.PhoneticName == _custNameKana;
        }

        return custTable;
    }`
    - **Method:**
      - **Name:** `findByLedgerDimension`
      - **Source:** `public static CustTable findByLedgerDimension(
        LedgerDimensionAccount  _ledgerDimension,
        boolean                 _forupdate = false,
        ConcurrencyModel        _concurrencyModel = ConcurrencyModel::Auto)
    {
        CustTable                           custTable;
        DimensionAttributeValueCombination  ledgerDimension;

        custTable.selectForUpdate(_forupdate);
        if (_forupdate  && _concurrencyModel != ConcurrencyModel::Auto)
        {
            custTable.concurrencyModel(_concurrencyModel);
        }

        select firstonly custTable
            join RecId from ledgerDimension where
                ledgerDimension.DisplayValue == custTable.AccountNum &&
                ledgerDimension.RecId == _ledgerDimension;

        return custTable;
    }`
    - **Method:**
      - **Name:** `findByPartyRecId`
      - **Source:** `[SysClientCacheDataMethodAttribute(true)]
    static CustTable findByPartyRecId(DirPartyRecId   _partyRecId,
                                   boolean _forUpdate = false)
    {
        CustTable custTable;

        if (_partyRecId)
        {
            if (_forUpdate)
            {
                custTable.selectForUpdate(_forUpdate);
            }

            select firstonly custTable
                where custTable.Party == _partyRecId;
        }
        return custTable;
    }`
    - **Method:**
      - **Name:** `findByRef`
      - **Source:** `/// <summary>
    /// Finds the specified record in the <c>CustTable</c> table.
    /// </summary>
    /// <param name="_refCompany">
    /// The reference company of the <c>CustTable</c> record to find.
    /// </param>
    /// <param name="_custTableRecId">
    /// The reference record ID of the <c>CustTable</c> record to find.
    /// </param>
    /// <param name="_forUpdate">
    /// A Boolean value that indicates whether to read the record for update; optional.
    /// </param>
    /// <returns>
    /// A record from the <c>CustTable</c> table; otherwise, an empty record.
    /// </returns>
    public static CustTable findByRef(
        CompanyId _refCompany,
        RecId _custTableRecId,
        boolean _forUpdate = false)
    {
        CustTable custTable;

        if (_refCompany != '' && _custTableRecId != 0)
        {
            custTable.selectForUpdate(_forUpdate);

            select firstonly crossCompany custTable
                where custTable.DataAreaId == _refCompany &&
                    custTable.RecId == _custTableRecId ;
        }

        return custTable;
    }`
    - **Method:**
      - **Name:** `findByVendor`
      - **Source:** `/// <summary>
    /// Finds the specified record in the <c>CustTable</c> table.
    /// </summary>
    /// <param name="_vendAccount">
    /// The vendor account of the <c>CustTable</c> record to find.
    /// </param>
    /// <param name="_forUpdate">
    /// A Boolean value that indicates whether to read the record for update; optional.
    /// </param>
    /// <returns>
    /// A record from the <c>CustTable</c> table; otherwise, an empty record.
    /// </returns>
    public static CustTable findByVendor(VendAccount _vendAccount, boolean _forUpdate = false)
    {
        CustTable custTable;

        if (_vendAccount)
        {
            custTable.selectForUpdate(_forUpdate);

            select firstonly custTable
                where custTable.VendAccount == _vendAccount;
        }

        return custTable;
    }`
    - **Method:**
      - **Name:** `updateWorkflowState`
      - **Source:** `/// <summary>
    /// Updates the state of the workflow field to a given value.
    /// </summary>
    /// <param name = "_recId">The ID of the record to update.</param>
    /// <param name = "_state">The desired state.</param>
    public static void updateWorkflowState(RecId _recId, CustTableChangeProposalWorkflowState _state)
    {
        ttsbegin;
        CustTable custTable = CustTable::findRecId(_recId, true);
        custTable.WorkflowState = _state;
        CustTable.update();
        ttscommit;
    }`
    - **Method:**
      - **Name:** `findRecId`
      - **Source:** `static CustTable findRecId(RecId    _recId,
                               boolean  _forUpdate = false)
    {
        CustTable custTable;

        if (_recId)
        {
            custTable.selectForUpdate(_forUpdate);

            select firstonly custTable
                where custTable.RecId == _recId;
        }

        return custTable;
    }`
    - **Method:**
      - **Name:** `isDelayNumberSequenceGeneration`
      - **Source:** `/// <summary>
    /// Determine if number sequence generation for customer account number should be delayed.
    /// </summary>
    /// <returns> True, if the number sequence generation for customer account number should be delayed; false, otherwise. </returns>
    public static boolean isDelayNumberSequenceGeneration()
    {
        // Delay the number sequence generation for account number
        // if there are account number sequence assignment on any cust group or the account number is tied to a global number sequence on AR parameters page.
        if (CustGroup::doesAnyGroupHaveCustAccountNumberSequenceSet() || CustVendCopyDataUtil::isCustAccountNumSequenceGlobal(curExt()))
        {
            return true;
        }
        else
        {
            return false;
        }
    }`
    - **Method:**
      - **Name:** `getDelayNumberSequenceGenerationInfoMessage`
      - **Source:** `/// <summary>
    /// Retrieve the message that indicate the number sequence generation for customer account number will be delayd.
    /// </summary>
    /// <returns> The message that indicate the number sequence generation for customer account number will be delayd. </returns>
    public static str getDelayNumberSequenceGenerationInfoMessage()
    {
        return "@AccountsReceivable:DelayCustAccountNumberGeneration";
    }`
    - **Method:**
      - **Name:** `getNumberSeqFormHandler`
      - **Source:** `/// <summary>
    /// Retrieves the numbersequenceformhandler that can be used in the form creation of this entity
    /// to generate the numbersequence related field
    /// </summary>
    /// <param name="callerForm">
    /// The form that will be using this numbersequenceformhandler to create this entity
    /// </param>
    /// <param name="numberSequenceDatasource">
    /// The datasource that contains this entity on the creation form
    /// </param>
    /// <returns>
    /// the numbersequenceformhandler that can be used in the form creation of this entity
    /// </returns>
    public static NumberSeqFormHandler getNumberSeqFormHandler(ObjectRun callerForm, FormDataSource numberSequenceDatasource)
    {
        NumberSeqFormHandler numberSeqFormHandler;
        FormRun callerFormRun = callerForm as FormRun;
        FormDataSource custDatasource = callerFormRun.dataSource(1);
        CustTable cust = custDatasource.cursor();
        CustGroup custGroup = CustGroup::find(cust.CustGroup);

        DirNumberSequenceIProvider numberSequenceProvider = callerFormRun as DirNumberSequenceIProvider;

        if (numberSequenceProvider)
        {
            numberSeqFormHandler = numberSequenceProvider.getCurNumberSeqFormHandler();
        }

        if (custGroup.CustAccountNumSeq)
        {
            if (!numberSeqFormHandler || numberSeqFormHandler.parmNumberSequenceId() != custGroup.CustAccountNumSeq)
            {
                numberSeqFormHandler = NumberSeqFormHandler::newForm(custGroup.CustAccountNumSeq,
                                                                    callerForm,
                                                                    numberSequenceDatasource,
                                                                    fieldNum(CustTable, AccountNum)
                                                                );
            }
        }
        else
        {
            if (!numberSeqFormHandler || numberSeqFormHandler.parmNumberSequenceId() != CustParameters::numRefCustAccount().NumberSequenceId)
            {
                numberSeqFormHandler = NumberSeqFormHandler::newForm(CustParameters::numRefCustAccount().NumberSequenceId,
                                                                    callerForm,
                                                                    numberSequenceDatasource,
                                                                    fieldNum(CustTable, AccountNum)
                                                                );
            }
        }

        return numberSeqFormHandler;
    }`
    - **Method:**
      - **Name:** `groupId`
      - **Source:** `static CustGroupId groupId(CustAccount _custAccount)
    {
        return CustTable::find(_custAccount).CustVendTable::groupId();
    }`
    - **Method:**
      - **Name:** `inventSiteIdCustItem`
      - **Source:** `/// <summary>
    /// Looks up site ID for an item depending on the customer if multisite is enabled.
    /// </summary>
    /// <param name="_custAccount">
    /// The customer buying the item.
    /// </param>
    /// <param name="_itemId">
    /// The ID of the item.
    /// </param>
    /// <returns>
    /// The site from which the item is sold to the customer.
    /// </returns>
    static InventSiteId inventSiteIdCustItem(CustAccount _custAccount, ItemId _itemId)
    {
        InventItemSalesSetup inventItemSalesSetup = InventItemSalesSetup::findDefault(_itemId);
        CustTable            custTable;
        InventParameters     inventParameters;
        InventSiteId         inventSiteIdItem;

        if (inventItemSalesSetup)
        {
            inventSiteIdItem = inventItemSalesSetup.inventSiteId();

            if (inventItemSalesSetup.MandatoryInventSite)
            {
                return inventSiteIdItem;
            }
        }

        custTable = CustTable::find(_custAccount);

        if (custTable.InventSiteId)
        {
            return custTable.InventSiteId;
        }

        if (inventSiteIdItem)
        {
            return inventSiteIdItem;
        }

        inventParameters = InventParameters::find();

        if (inventParameters.fallbackSiteId())
        {
            return inventParameters.fallbackSiteId();
        }

        return '';
    }`
    - **Method:**
      - **Name:** `isCustDKPublic`
      - **Source:** `/// <summary>
    /// Determines whether sending invoice electronically is applicable for specified customer or not.
    /// </summary>
    /// <param name="_invoiceAccount">
    /// Customer account.
    /// </param>
    /// <returns>
    /// Yes if  sending invoice electronically is applicable for specified customer otherwise No.
    /// </returns>
    public static NoYes isCustDKPublic(CustInvoiceAccount _invoiceAccount)
    {
        CustTable custTable;
        boolean     ret = false;

        #isoCountryRegionCodes

        custTable = CustTable::find(_invoiceAccount);

        if (custTable.eInvoice == NoYes::Yes)
        {
            if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoDK]))
            {
                if (custTable.EinvoiceEANNum)
                {
                    ret = true;
                }
            }
            else
            {
                ret = true;
            }
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `jumpRefCustomer`
      - **Source:** `static void jumpRefCustomer(CustVendAC  _accountNum, CompanyId _companyId  = curext())
    {
        var companyId = _companyId;
        if (! xDataArea::exist(companyId))
        {
            throw error(strFmt("@SYS10666", companyId));
        }

        changecompany(companyId)
        {
            CustTable::find(_accountNum).jumpRef();
        }
    }`
    - **Method:**
      - **Name:** `jumpRef`
      - **Source:** `public void jumpRef()
    {
        var args = new Args();
        args.record(this);

        new MenuFunction(menuitemDisplayStr(CustTable), MenuItemType::Display).run(args);
    }`
    - **Method:**
      - **Name:** `lookupCustomer`
      - **Source:** `public static void lookupCustomer(FormControl _formControl, CompanyId _companyId  = curExt(), CustTableLookupDisplayType _displayType = CustTableLookupDisplayType::Default)
    {
        // The following block is needed for the case that intercompany transactions is disabled and someone passes
        // in an empty string. Ideally one would remove the "curext()" defaulting from the paramter list, but this
        // code was added as part of a bug fix, and did not have the scope or need of changing the interface.
        var companyId = _companyId ? _companyId : curExt();
        if (!xDataArea::exist(companyId))
        {
            throw error(strFmt("@SYS10666", companyId));
        }

        changecompany(companyId)
        {
            Args args = CustTable::setupArgsForCustomerLookup(_formControl);

            var formRun = classfactory.formRunClass(args);
            formRun.init();

            CustTable::executeCustomerLookup(_formControl, formRun);
        }
    }`
    - **Method:**
      - **Name:** `setupArgsForCustomerLookup`
      - **Source:** `/// <summary>
    /// Sets up an <c>Args</c> object to be used for creating the customer lookup.
    /// </summary>
    /// <param name = "_formControl">The <c>FormControl</c> storing the customer.</param>
    /// <param name = "_displayType">The <c>CustTableLookupDisplayType</c> enumeration value.</param>
    /// <returns>An <c>Args</c> object to be used for creating the customer lookup.</returns>
    protected static Args setupArgsForCustomerLookup(FormControl _formControl, CustTableLookupDisplayType _displayType = CustTableLookupDisplayType::Default)
    {
        Args args = new Args();
        args.name(formStr(CustTableLookup));
        args.caller(_formControl);
        args.lookupField(fieldNum(CustTable, AccountNum));

        FormStringControl formStringControl;
        FormSegmentedEntryControl segmentedEntryControl;
        if (_formControl is FormStringControl)
        {
            formStringControl = _formControl as FormStringControl;
            args.lookupValue(formStringControl.text());
        }
        else if (_formControl is SegmentedEntryControl)
        {
            segmentedEntryControl = _formControl as SegmentedEntryControl;
            args.lookupValue(segmentedEntryControl.valueStr());
        }
        else
        {
            throw error(Error::wrongUseOfFunction(funcName()));
        }

        // specify the display type of the lookup
        args.parmEnumType(enumNum(CustTableLookupDisplayType));
        args.parmEnum(_displayType);

        return args;
    }`
    - **Method:**
      - **Name:** `executeCustomerLookup`
      - **Source:** `/// <summary>
    /// Executes the customer lookup for the given <c>FormControl</c> instance.
    /// </summary>
    /// <param name = "_formControl">The <c>FormControl</c> storing the customer.</param>
    /// <param name = "_formRun">The lookup form to be executed.</param>
    protected static void executeCustomerLookup(FormControl _formControl, FormRun _formRun)
    {
        if (_formControl is FormStringControl)
        {
            FormStringControl formStringControl = _formControl as FormStringControl;
            formStringControl.performFormLookup(_formRun);
        }
        else if (_formControl is SegmentedEntryControl)
        {
            SegmentedEntryControl segmentedEntryControl = _formControl as SegmentedEntryControl;
            segmentedEntryControl.performFormLookup(_formRun);
        }
        else
        {
            throw error(Error::wrongUseOfFunction(funcName()));
        }
    }`
    - **Method:**
      - **Name:** `resolveAmbiguousReferenceCustomer`
      - **Source:** `/// <summary>
    /// Resolves the user's entered value, either by taking the value directly as the customer account number or by mapping
    /// it to the customer name, which allows the account number value to be found indirectly.
    /// </summary>
    /// <param name = "_formControl">The control on which contextual data entry is being performed.</param>
    /// <param name="_companyId">The company within which the contextual lookup should be performed; optional.</param>
    /// <returns>The resolved value.</returns>
    /// <remarks>
    /// This method is designed to be used in conjuction with the <c>CustTable::lookupCustomer</c> method.
    /// </remarks>
    public static str resolveAmbiguousReferenceCustomer(FormControl _formControl, CompanyId _companyId = curExt())
    {
        var companyId = _companyId ? _companyId : curExt();
        if (!xDataArea::exist(companyId))
        {
            throw error(strFmt("@SYS10666", companyId));
        }

        changecompany(companyId)
        {
            return CustomerDataInteractorFactory::resolveAmbiguousReferenceForControl(_formControl);
        }
    }`
    - **Method:**
      - **Name:** `mcrMergeCustomers`
      - **Source:** `/// <summary>
    /// Merges the passed customers together.
    /// </summary>
    /// <param name="_mergeFromCust">
    /// The <c>CustAccount</c> object being merged into another customer.
    /// </param>
    /// <param name="_mergeToCust">
    /// The <c>CustAccount</c> object that the from customer account is being merged to.
    /// </param>
    /// <returns>
    /// The results of the merged <c>CustAccount</c> records.
    /// </returns>
    public static CustTable mcrMergeCustomers(CustAccount _mergeFromCust, CustAccount _mergeToCust)
    {
        MCRMergeConfirmationConfigure   mergConfirmationConfigure = new MCRMergeConfirmationConfigure();
        Args                            args;
        FormRun                         formRun;
        CustTable                       custTableReturn;

        // error checking that two records are selected
        if (!_mergeFromCust || !_mergeToCust)
        {
            error("@MCR12295");
        }
        else
        {
            mergConfirmationConfigure.parmMergedFromCust(_mergeFromCust);
            mergConfirmationConfigure.parmMergedToCust(_mergeToCust);

            args = new Args();
            args.name(formStr(MCRMergeConfirmationForm));
            args.parmObject(mergConfirmationConfigure);

            formRun = classfactory.formRunClass(args);
            formRun.init();
            formRun.run();
            formRun.wait();

            if (args.parm())
            {
                custTableReturn = CustTable::find(args.parm());
            }
        }

        return custTableReturn;
    }`
    - **Method:**
      - **Name:** `projPriceGroup`
      - **Source:** `static ProjPriceGroupID projPriceGroup(CustAccount _custAccount)
    {
        if (_custAccount)
        {
            return (select firstonly PriceGroup from custTable
                    where custTable.AccountNum == _custAccount).PriceGroup;
        }

        return "";
    }`
    - **Method:**
      - **Name:** `promptAddress`
      - **Source:** `static str promptAddress(CustAccount _custAccount, LogisticsLocationRoleType _roleType = LogisticsLocationRoleType::None)
    {
        CustTable                  custTable   = CustTable::find(_custAccount);
        LogisticsPostalAddress     address     = LogisticsLocationEntity::findPostalAddress(custTable, _roleType);

        if (address)
        {
            return (custTable.name() + '\n' + address.Address);
        }

        return (custTable.name() + '\n' + custTable.postalAddress().Address);
    }`
    - **Method:**
      - **Name:** `promptConvertCurrencyCode`
      - **Source:** `static boolean promptConvertCurrencyCode(CustAccount _custAccount)
    {
        return (Box::yesNo("@SYS81865", DialogButton::Yes) == DialogButton::Yes);
    }`
    - **Method:**
      - **Name:** `quickCreateSaveAndOpenLinks`
      - **Source:** `public static container quickCreateSaveAndOpenLinks()
    {
        container saveAndOpenLinks;

        saveAndOpenLinks += [menuitemActionStr(SalesCreateQuotation), MenuItemType::Action];

        // Retail SMB doesn't support project quotation
        if (!RetailSMB::IsRetailSMBEnabled())
        {
            saveAndOpenLinks += [menuitemDisplayStr(SalesQuotationProjTableForNew), MenuItemType::Display];
        }

        saveAndOpenLinks += [menuitemActionStr(SalesCreateOrderFromCustomer), MenuItemType::Action];

        return saveAndOpenLinks;
    }`
    - **Method:**
      - **Name:** `retreiveQuickCreateSettings`
      - **Source:** `/// <summary>
    /// Retrieves the needed metadata for the quick create form
    /// </summary>
    /// <returns>
    /// Container with needed metadata for quick create form
    /// </returns>
    public static container retreiveQuickCreateSettings()
    {
        container quickCreateSettings;
        quickCreateSettings += [CustTable::quickCreateSaveAndOpenLinks()];

        return quickCreateSettings;
    }`
    - **Method:**
      - **Name:** `txtNotExist`
      - **Source:** `static TxtNotExist txtNotExist()
    {
        return "@SYS9779";
    }`
    - **Method:**
      - **Name:** `DirPartyTable_FK`
      - **Source:** `public DirPartyTable DirPartyTable_FK(DirPartyTable _relatedTable = null)
    {
        if (prmIsDefault(_relatedTable))
        {
            return this.setLink('DirPartyTable_FK');
        }
        else
        {
            return this.setLink('DirPartyTable_FK', _relatedTable);
        }
    }`
    - **Method:**
      - **Name:** `findInvoiceAddressAccountFromParty`
      - **Source:** `/// <summary>
    /// Finds the customer invoice account entry for a given party.
    /// </summary>
    /// <param name="_partyRecId">
    /// The party RecID to find the customer invoice account entry.
    /// </param>
    /// <param name="_forUpdate">
    /// Whether the record should be select for update; optional.
    /// </param>
    /// <returns>
    /// The customer invoice account for the given party RecID.
    /// </returns>
    public static CustTable findInvoiceAddressAccountFromParty(DirPartyRecId _partyRecId, boolean _forUpdate = false)
    {
        CustTable   custTable;

        custTable = CustTable::findByPartyRecId(_partyRecId, _forUpdate);
        if (custTable.InvoiceAddress == InvoiceOrderAccount::InvoiceAccount &&
            custTable.InvoiceAccount &&
            custTable.AccountNum != custTable.InvoiceAccount)
        {
            custTable = CustTable::find(custTable.InvoiceAccount, _forUpdate);
        }

        return custTable;
    }`
    - **Method:**
      - **Name:** `DocuRefOnInsert`
      - **Source:** `[SubscribesTo(classstr(DocuRefExtension), delegatestr(DocuRefExtension, OnInsert))]
    static void DocuRefOnInsert(DocuRef _docuRef, RecId _interCompanyFromRecId)
    {
        if (_docuRef.RefTableId == tablenum(CustTable))
        {
            changecompany(_docuRef.RefCompanyId)
            {
                _docuRef.Party         = CustTable::findRecId(_docuRef.RefRecId).Party;
                _docuRef.Author        = DirPersonUser::current().PersonParty;
                _docuRef.ActualCompanyId = curext();
            }
        }
    }`
    - **Method:**
      - **Name:** `editInvReportFormat`
      - **Source:** `/// <summary>
    /// Sets the report format in print setting of the Customer
    /// </summary>
    /// <param name="_set">
    /// boolean value true/ false if format changed
    /// </param>
    /// <param name="_newReportFormat">
    /// new report format selected
    /// </param>
    /// <returns>
    /// returns report format
    /// </returns>
    public edit ProjInvReportFmtDesc editInvReportFormat(boolean _set, PrintMgmtReportFormatDescription  _newReportFormat)
    {
        PrintMgmtReportFormatDescription    newReportFormat = _newReportFormat;
        PrintMgmtReportFormatDescription    reportFormat;

        reportFormat = ProjInvoicePrintMgmt::getReportFormat(this);
        if (_set)
        {
            if (this.RecId && newReportFormat && newReportFormat != reportFormat)
            {
                ProjInvoicePrintMgmt::createOrUpdateInvoicePrintSettings(this,PrintMgmtNodeType::CustTable,newReportFormat);
                reportFormat = newReportFormat;
            }
        }
        return reportFormat;
    }`
    - **Method:**
      - **Name:** `getDefaultTaxInformation_TH`
      - **Source:** `/// <summary>
    ///  Get default tax information of current customer's invoice address.
    /// </summary>
    /// <returns>
    /// The tax information from customer's invoice address.
    /// </returns>
    /// <remarks>
    /// If customer has no invoice address, the primary address will be returned.
    /// </remarks>
    public TaxInformation_TH getDefaultTaxInformation_TH()
    {
        TaxInformation_TH       taxInformation_TH;
        LogisticsPostalAddress  invoiceAddress;

        invoiceAddress = this.invoiceAddress();

        if (invoiceAddress)
        {
            select firstonly taxInformation_TH
                where taxInformation_TH.LogisticsLocation == invoiceAddress.Location;
        }

        return taxInformation_TH;
    }`
    - **Method:**
      - **Name:** `getDefaultTaxRegistration_TH`
      - **Source:** `/// <summary>
    ///  Get default tax registration of current customer's invoice address.
    /// </summary>
    /// <param name="_transDate">
    /// The effective date of tax registration, default is today.
    /// </param>
    /// <returns>
    /// The tax registration from customer's invoice address.
    /// </returns>
    /// <remarks>
    /// If customer has no invoice address, the primary address will be returned.
    /// </remarks>
    public TaxRegistration getDefaultTaxRegistration_TH(TransDate _transDate = DateTimeUtil::getSystemDate(DateTimeUtil::getUserPreferredTimeZone()))
    {
        DirPartyLocation                        dirPartyLocation;
        TaxRegistration                         taxRegistration;
        TaxRegistrationTypeApplicabilityRule    taxRegistrationTypeApplicabilityRule;

        if (_transDate)
        {
            dirPartyLocation = DirPartyLocation::findByPartyLocation(
                this.Party,
                this.invoiceAddress().Location);

            if (dirPartyLocation)
            {
                select firstonly validTimeState(_transDate) taxRegistration
                    where taxRegistration.DirPartyLocation == dirPartyLocation.RecId
                    join RecId from taxRegistrationTypeApplicabilityRule
                        order by taxRegistrationTypeApplicabilityRule.IsPrimaryAddressRestricted desc
                        where taxRegistration.TaxRegistrationTypeApplicabilityRule == taxRegistrationTypeApplicabilityRule.RecId;
            }
        }

        return taxRegistration;
    }`
    - **Method:**
      - **Name:** `isForeigner_BR`
      - **Source:** `/// <summary>
    /// Indicates whether a customer is identified as foreigner based on primary postal address and company
    /// </summary>
    /// <returns>
    /// true if a customer is a foreigner; otherwise, false.
    /// </returns>
    public boolean isForeigner_BR()
    {
        boolean isForeigner = false;

        if (this.postalAddress().CountryRegionId != CompanyInfo::find().postalAddress().CountryRegionId)
        {
            isForeigner = true;
        }

        return isForeigner;
    }`
    - **Method:**
      - **Name:** `getCollectionsContactPerson`
      - **Source:** `/// <summary>
    /// Returns the collections contact person.
    /// </summary>
    /// <returns>Returns a <c>ContactPerson</c> record.</returns>
    public ContactPerson getCollectionsContactPerson()
    {
        var contact = CustCollectionsContact::find(this.AccountNum);
        ContactPerson contactPersonCollections;

        // Try to use the Collections Contact first...
        if (contact.hasContactPerson())
        {
            select contactPersonCollections
                where contactPersonCollections.ContactPersonId == contact.ContactPersonId;
        }

        // ...if there is none, try to use the customer's general contact person...
        if (!contactPersonCollections)
        {
            select firstonly contactPersonCollections
                order by contactPersonCollections.ContactPersonId
                where contactPersonCollections.ContactPersonId == this.ContactPersonId;
        }

        // ...if there is none, as a last resort, use the first person by person ID
        if (!contactPersonCollections)
        {
            CustTable custTable;

            select firstonly contactPersonCollections
                order by contactPersonCollections.ContactPersonId
                exists join custTable
                where custTable.AccountNum                      == this.AccountNum
                   && contactPersonCollections.ContactForParty  == custTable.Party;
        }

        return contactPersonCollections;
    }`
    - **Method:**
      - **Name:** `isCustPublic_NO`
      - **Source:** `/// <summary>
    ///   Checks if customer is Norwegian public sector company. Required for E-Invoice processing.
    /// </summary>
    /// <param name="_custAccount">
    ///   Customer account.
    /// </param>
    /// <returns>
    ///   True, if customer is norwegian public sector company; Otherwise, false.
    /// </returns>
    /// <remarks>
    ///   The method partially duplicates isCustDKPublic method functionality. Its required for handing several Norway specific fields, in other cases isCustDKPublic used.
    /// </remarks>
    public static boolean isCustPublic_NO(CustAccount _custAccount)
    {
        #isoCountryRegionCodes

        boolean ret;

        if (SysCountryRegionCode::isLegalEntityInCountryRegion([#isoNO]) && CustTable::find(_custAccount).EInvoice)
        {
            ret = true;
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `isCustomerDefaultForRetail`
      - **Source:** `/// <summary>
    /// Check if customer is set to the default customer for a retail channel.
    /// </summary>
    /// <param name = "_custAccount">Customer account.</param>
    /// <returns>True, if customer is default for retail channel; Otherwise, false.</returns>
    internal static boolean isCustomerDefaultForRetail(CustAccount _custAccount)
    {
        boolean ret = false;
        RetailChannelTable retailChannelTable;
        RetailStoreTable retailStoreTable;
        RetailMCRChannelTable retailMCRChannelTable;

        select firstonly RecId from retailChannelTable
            where retailChannelTable.DefaultCustAccount == _custAccount;

        if (retailChannelTable.RecId)
        {
            ret = true;
        }
        else
        {
            select firstonly RecId from retailStoreTable
                where retailStoreTable.DefaultCustAccount == _custAccount;

            if (retailStoreTable.RecId)
            {
                ret = true;
            }
            else
            {
                select firstonly RecId from retailMCRChannelTable
                    where retailMCRChannelTable.DefaultCustAccount == _custAccount;

                if (retailMCRChannelTable.RecId)
                {
                    ret = true;
                }
            }
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `constructAmbiguousReferenceResolverEvent`
      - **Source:** `[SubscribesTo(classStr(CaseAssociationLinkDataInteractorFactory), delegatestr(CaseAssociationLinkDataInteractorFactory, constructAmbiguousReferenceResolverEvent))]
    public static void constructAmbiguousReferenceResolverEvent(caseAssociationLinkInteractorFactoryEventArgs _args, DataInteractorTarget _dataInteractorTarget, Common _boundRecord)
    {
        _args.parmAmbiguousReferenceResolver(AmbiguousReferenceResolver::construct(_dataInteractorTarget, CustomerDataInteractorFactory::PkFieldBinding));
    }`
    - **Method:**
      - **Name:** `canModifyChangeProposal`
      - **Source:** `/// <summary>
    /// Determines whether the workflow is in such a state that changes can be made to change proposal fields.
    /// </summary>
    /// <returns>True if changes can be made; otherwise false.</returns>
    public boolean canModifyChangeProposal()
    {
        boolean ret = false;
        if (this.WorkflowState == CustTableChangeProposalWorkflowState::NotSubmitted
            || this.WorkflowState == CustTableChangeProposalWorkflowState::Rejected)
        {
            ret = true;
        }

        return ret;
    }`
    - **Method:**
      - **Name:** `DimensionAttributeDelegates_getTablesToAddCopiedValuesTo`
      - **Source:** `/// <summary>
    /// Gets a list of tables and fields to update when copying the values to default dimensions on existing values.
    /// </summary>
    /// <param name = "_tableSet">A <c>Set</c> of <c>DimensionCopyValuesDataContract</c> values.</param>
    [SubscribesTo(classStr(DimensionAttributeDelegates), delegateStr(DimensionAttributeDelegates, getTablesToAddCopiedValuesTo))]
    public static void DimensionAttributeDelegates_getTablesToAddCopiedValuesTo(Set _tableSet)
    {
        DimensionCopyValueDataContract contract = DimensionCopyValueDataContract::construct(tableNum(CustTable), fieldNum(CustTable, DefaultDimension));
        contract.addKeyFieldDimensionPair(tableNum(CustTable), fieldNum(CustTable, AccountNum));
        _tableSet.add(contract);
    }`
    - **Method:**
      - **Name:** `getRegistrationNumber_AE`
      - **Source:** `/// <summary>
    /// This method will be used for getting TRN registration number
    /// </summary>
    /// <returns>
    /// Returns trn registration number for UAE of given customer account
    /// </returns>
    public VATNum getRegistrationNumber_AE()
    {
        #ISOCountryRegionCodes
        TaxRegistration taxRegistration;
        TaxAuthorityAddress taxAuthorityAddress;
        
        select firstonly TaxAuthority from taxAuthorityAddress
            where taxAuthorityAddress.DataAreaId == curExt();

        if (taxAuthorityAddress.TaxAuthority)
        {
            DirPartyLocation dirPartyLocation;
            LogisticsPostalAddress logisticsPostalAddress;
            TaxRegistrationTypeApplicabilityRule taxRegistrationTypeApplicabilityRule;
            LogisticsAddressCountryRegion taxlogisticsAddressCountryRegion;
            LogisticsAddressCountryRegion logisticsAddressCountryRegion;
            TaxRegistrationType taxRegistrationType;
            LogisticsAddressCountryRegionISOCode isoCode = #isoAE;

            select firstonly RegistrationNumber from taxRegistration
                exists join dirPartyLocation
                    where dirPartyLocation.RecId == taxRegistration.DirPartyLocation
                        && dirPartyLocation.Party == this.Party
                        && dirPartyLocation.IsPrimaryTaxRegistration == NoYes::Yes
                exists join logisticsPostalAddress
                    where logisticsPostalAddress.Location == dirPartyLocation.Location
                exists join logisticsAddressCountryRegion
                    where logisticsAddressCountryRegion.CountryRegionId == logisticsPostalAddress.CountryRegionId
                        && logisticsAddressCountryRegion.ISOcode == isoCode
                exists join taxRegistrationTypeApplicabilityRule
                    where taxRegistrationTypeApplicabilityRule.RecId == taxRegistration.TaxRegistrationTypeApplicabilityRule
                exists join taxRegistrationType
                    where taxRegistrationTypeApplicabilityRule.TaxRegistrationType == taxRegistrationtype.RecId
                        && taxRegistrationTypeApplicabilityRule.TaxRegistrationAuthority== taxAuthorityAddress.TaxAuthority
                exists join taxlogisticsAddressCountryRegion
                    where taxlogisticsAddressCountryRegion.CountryRegionId == taxRegistrationTypeApplicabilityRule.CountryRegionId
                        && taxlogisticsAddressCountryRegion.ISOcode == isoCode
                        && taxRegistration.ValidTo >= DateTimeUtil::getToday(DateTimeUtil::getCompanyTimeZone());
        }

        return taxRegistration.RegistrationNumber;
    }`
    - **Method:**
      - **Name:** `collectionLetterNum`
      - **Source:** `/// <summary>
    /// Gets the last collection letter number for this customer.
    /// </summary>
    /// <returns>The last collection letter number for this customer.</returns>
    [SysClientCacheDataMethod(false)]
    public display CollectionLetterNum collectionLetterNum()
    {
        return CustCollectionLetterJour::lastUpdatedCollectionLetterPerCustomer(this.AccountNum).CollectionLetterNum;
    }`
    - **Method:**
      - **Name:** `collectionLetterCode`
      - **Source:** `/// <summary>
    /// Gets the last collection letter code for this customer.
    /// </summary>
    /// <returns>The last collection letter code for this customer.</returns>
    [SysClientCacheDataMethod(false)]
    public display CustCollectionLetterCode collectionLetterCode()
    {
        return CustCollectionLetterJour::lastUpdatedCollectionLetterPerCustomer(this.AccountNum).CollectionLetterCode;
    }`
    - **Method:**
      - **Name:** `collectionLetterDate`
      - **Source:** `/// <summary>
    /// Gets the last collection letter date for this customer.
    /// </summary>
    /// <returns>The last collection letter date for this customer.</returns>
    [SysClientCacheDataMethod(false)]
    public display TransDate collectionLetterDate()
    {
        return CustCollectionLetterJour::lastUpdatedCollectionLetterPerCustomer(this.AccountNum).CollectionLetterDate;
    }`
    - **Method:**
      - **Name:** `editCollectionLetterCode`
      - **Source:** `/// <summary>
    /// Gets the valid collection letter code for this customer.  This is determined by comparing the last posted collection letter code
    /// and a collection letter code set for this customer, and returning the lower value.
    /// </summary>
    /// <param name = "_set">Indicates if the caller is passing a new value to assign to this customer.</param>
    /// <param name = "_collectionLetterCode">A new value to assign to this customer, if indicated by the first parameter.</param>
    /// <returns>The lower of the last collection letter code and the code manually set for this customer.</returns>
    [SysClientCacheDataMethod(false)]
    public edit CustCollectionLetterCode editCollectionLetterCode(boolean _set, CustCollectionLetterCode _collectionLetterCode)
    {
        CustCollectionLetterCodeOrderedList custCollectionLetterCodeOrderedList = CustCollectionLetterCodeOrderedList::newFromOrder();
        CustCollectionLetterCode localCollectionLetterCode = this.collectionLetterCode();
        
        if (_set)
        {
            if (custCollectionLetterCodeOrderedList.indexOf(_collectionLetterCode) > custCollectionLetterCodeOrderedList.indexOf(localCollectionLetterCode))
            {
                checkFailed(strFmt("@SYS77955", _collectionLetterCode));
            }
            else
            {
                this.CollectionLetterCode = _collectionLetterCode;
            }
        }

        if (custCollectionLetterCodeOrderedList.indexOf(this.CollectionLetterCode) < custCollectionLetterCodeOrderedList.indexOf(localCollectionLetterCode))
        {
            localCollectionLetterCode = this.CollectionLetterCode;
        }

        return localCollectionLetterCode;
    }`
- **ConfigurationKey:** `LedgerBasic`
- **DeveloperDocumentation:** `@SYS125115`
- **Label:** `@SYS11307`
- **TableGroup:** `Main`
- **TitleField1:** `AccountNum`
- **TitleField2:** `Party`
- **AllowRowVersionChangeTracking:** `Yes`
- **CacheLookup:** `Found`
- **ClusteredIndex:** `AccountIdx`
- **CreatedDateTime:** `Yes`
- **DataSharingType:** `Duplicate`
- **DisableLockEscalation:** `Yes`
- **ModifiedBy:** `Yes`
- **ModifiedDateTime:** `Yes`
- **Modules:** `Customer`
- **PrimaryIndex:** `AccountIdx`
- **ReplacementKey:** `AccountIdx`
- **DeleteActions:**
  - **AxTableDeleteAction:**
    - **Name:** `PlInventPackageReturn`
    - **DeleteAction:** `Restricted`
    - **Relation:**
    - **Table:** `PlInventPackageReturn`
  - **AxTableDeleteAction:**
    - **Name:** `RetailCustTable`
    - **DeleteAction:** `Cascade`
    - **Relation:**
    - **Table:** `RetailCustTable`
  - **AxTableDeleteAction:**
    - **Name:** `MCRTargetList`
    - **DeleteAction:** `Restricted`
    - **Relation:**
    - **Table:** `MCRTargetList`
  - **AxTableDeleteAction:**
    - **Name:** `DeleteAction1`
    - **DeleteAction:** `Cascade`
    - **Relation:**
    - **Table:** `MCRItemListTable`
    - **Tags:**
  - **AxTableDeleteAction:**
    - **Name:** `DeleteAction2`
    - **DeleteAction:** `Cascade`
    - **Relation:**
    - **Table:** `CustCollectionsContact`
    - **Tags:**
  - **AxTableDeleteAction:**
    - **Name:** `CustAging`
    - **DeleteAction:** `Cascade`
    - **Relation:**
    - **Table:** `CustAging`
    - **Tags:**
  - **AxTableDeleteAction:**
    - **Name:** `DeleteAction3`
    - **DeleteAction:** `Cascade`
    - **Relation:** `CustTable`
    - **Table:** `CustAutomationManualAssignment`
    - **Tags:**
- **FieldGroups:**
  - **AxTableFieldGroup:**
    - **Name:** `AutoSummary`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvTerm`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymTermId`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSched`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditRating`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditMax`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesDistrictId`
      - **AxTableFieldGroupField:**
        - **DataField:** `LineOfBusinessId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SubsegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyChainId`
      - **AxTableFieldGroupField:**
        - **DataField:** `Blocked`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `EndDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustClassificationId`
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountStatement`
      - **AxTableFieldGroupField:**
        - **DataField:** `StatisticsGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `FreightZone`
  - **AxTableFieldGroup:**
    - **Name:** `AutoBrowse`
    - **Fields:**
  - **AxTableFieldGroup:**
    - **Name:** `AutoReport`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `Party`
      - **AxTableFieldGroupField:**
        - **DataField:** `OrgId`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
  - **AxTableFieldGroup:**
    - **Name:** `AutoLookup`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `Party`
      - **AxTableFieldGroupField:**
        - **DataField:** `OurAccountNum`
  - **AxTableFieldGroup:**
    - **Name:** `AutoIdentification`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Party`
  - **AxTableFieldGroup:**
    - **Name:** `AddressLookup`
    - **Label:** `@SYS88672`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
  - **AxTableFieldGroup:**
    - **Name:** `Administration`
    - **Label:** `@SYS9853`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Blocked`
      - **AxTableFieldGroupField:**
        - **DataField:** `OneTimeCustomer`
      - **AxTableFieldGroupField:**
        - **DataField:** `StatisticsGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountStatement`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForecastDMPInclude`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustExcludeCollectionFee`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustExcludeInterestCharges`
  - **AxTableFieldGroup:**
    - **Name:** `AdministrationEP`
    - **Label:** `@SYS9853`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Blocked`
      - **AxTableFieldGroupField:**
        - **DataField:** `OneTimeCustomer`
      - **AxTableFieldGroupField:**
        - **DataField:** `StatisticsGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountStatement`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForecastDMPInclude`
  - **AxTableFieldGroup:**
    - **Name:** `Affiliated_RU`
    - **Label:** `@SYS4002123`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Affiliated_RU`
  - **AxTableFieldGroup:**
    - **Name:** `All`
    - **Label:** `@SYS80094`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `usePurchRequest`
  - **AxTableFieldGroup:**
    - **Name:** `Budget`
    - **Label:** `@SYS15436`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ClearingPeriod`
  - **AxTableFieldGroup:**
    - **Name:** `CarrierInfo`
    - **Label:** `@SYS50722`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ShipCarrierId`
      - **AxTableFieldGroupField:**
        - **DataField:** `ShipCarrierAccountCode`
      - **AxTableFieldGroupField:**
        - **DataField:** `ShipCarrierAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `ShipCarrierBlindShipment`
  - **AxTableFieldGroup:**
    - **Name:** `CaseMoreInformation`
    - **Label:** `@SYS314356`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `name`
      - **AxTableFieldGroupField:**
        - **DataField:** `InvoiceAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
  - **AxTableFieldGroup:**
    - **Name:** `Classification`
    - **Label:** `@SYS81508`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustClassificationId`
  - **AxTableFieldGroup:**
    - **Name:** `CompanyChain`
    - **Label:** `@SYS105799`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyChainId`
  - **AxTableFieldGroup:**
    - **Name:** `ConnIntegration`
    - **Label:** `@SYS4004713`
    - **Fields:**
  - **AxTableFieldGroup:**
    - **Name:** `ConsDay_JP`
    - **Label:** `@GLS60066`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ConsDay_JP`
  - **AxTableFieldGroup:**
    - **Name:** `ContactInfo`
    - **Label:** `@SYS21663`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ContactPersonId`
      - **AxTableFieldGroupField:**
        - **DataField:** `editContactPersonName`
      - **AxTableFieldGroupField:**
        - **DataField:** `LineOfBusinessId`
      - **AxTableFieldGroupField:**
        - **DataField:** `BirthPlace_IT`
      - **AxTableFieldGroupField:**
        - **DataField:** `BirthCountyCode_IT`
      - **AxTableFieldGroupField:**
        - **DataField:** `ResidenceForeignCountryRegionId_IT`
      - **AxTableFieldGroupField:**
        - **DataField:** `AuthorityOffice_IT`
  - **AxTableFieldGroup:**
    - **Name:** `Credit`
    - **Label:** `@SYS7084`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `MandatoryCreditLimit`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditRating`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditMax`
  - **AxTableFieldGroup:**
    - **Name:** `CreditCardProcessing`
    - **Label:** `@SYS327526`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditCardAddressVerification`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditCardAddressVerificationVoid`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditCardAddressVerificationLevel`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditCardCVC`
  - **AxTableFieldGroup:**
    - **Name:** `Currency`
    - **Label:** `@sys7572`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
  - **AxTableFieldGroup:**
    - **Name:** `CustomerSelfService`
    - **Label:** `@SYS58654`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `WebSalesOrderDisplay`
  - **AxTableFieldGroup:**
    - **Name:** `Delivery`
    - **Label:** `@SYS4508`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `FreightZone`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvTerm`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvReason`
      - **AxTableFieldGroupField:**
        - **DataField:** `DestinationCodeId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesCalendarId`
      - **AxTableFieldGroupField:**
        - **DataField:** `ShipCarrierFuelSurcharge`
  - **AxTableFieldGroup:**
    - **Name:** `DeliveryTerms`
    - **Label:** `@SYS27703`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymTermId`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSpec`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSched`
      - **AxTableFieldGroupField:**
        - **DataField:** `CashDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `BankAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymDayId`
  - **AxTableFieldGroup:**
    - **Name:** `Dimension`
    - **Label:** `@SYS342338`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `DefaultDimension`
  - **AxTableFieldGroup:**
    - **Name:** `DirectDebit`
    - **Label:** `@SYS4002617`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `DefaultDirectDebitMandate`
  - **AxTableFieldGroup:**
    - **Name:** `Einvoice`
    - **Label:** `@SYS100769`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `EInvoice`
      - **AxTableFieldGroupField:**
        - **DataField:** `EinvoiceEANNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `EInvoiceRegister_IT`
      - **AxTableFieldGroupField:**
        - **DataField:** `EInvoiceAttachment`
  - **AxTableFieldGroup:**
    - **Name:** `EntryCertificate_W`
    - **Label:** `@SYS4004104`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `EntryCertificateRequired_W`
      - **AxTableFieldGroupField:**
        - **DataField:** `IssueOwnEntryCertificate_W`
  - **AxTableFieldGroup:**
    - **Name:** `EPCharacteristics`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustClassificationId`
      - **AxTableFieldGroupField:**
        - **DataField:** `Memo`
      - **AxTableFieldGroupField:**
        - **DataField:** `Party`
  - **AxTableFieldGroup:**
    - **Name:** `EPCharacteristics2`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SubsegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyChainId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesDistrictId`
      - **AxTableFieldGroupField:**
        - **DataField:** `MainContactWorker`
  - **AxTableFieldGroup:**
    - **Name:** `EPContactInfoAddEdit`
    - **Label:** `@SYS21663`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ContactPersonId`
      - **AxTableFieldGroupField:**
        - **DataField:** `editContactPersonName`
      - **AxTableFieldGroupField:**
        - **DataField:** `LineOfBusinessId`
  - **AxTableFieldGroup:**
    - **Name:** `EPCreditInfo`
    - **Label:** `@SYS7084`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `creditMaxCur`
      - **AxTableFieldGroupField:**
        - **DataField:** `balanceAllCurrency`
  - **AxTableFieldGroup:**
    - **Name:** `EPCustomer`
    - **Label:** `@SYS53631`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `Blocked`
  - **AxTableFieldGroup:**
    - **Name:** `EPDetailsOther`
    - **Label:** `@SYS316631`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ContactPersonId`
      - **AxTableFieldGroupField:**
        - **DataField:** `LineOfBusinessId`
      - **AxTableFieldGroupField:**
        - **DataField:** `MainContactWorker`
      - **AxTableFieldGroupField:**
        - **DataField:** `SegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SubsegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyChainId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesDistrictId`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `Memo`
  - **AxTableFieldGroup:**
    - **Name:** `EPFrench`
    - **Label:** `@SYS21663`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyIdSiret`
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyNAFCode`
  - **AxTableFieldGroup:**
    - **Name:** `EPGeneral3`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `MandatoryCreditLimit`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditRating`
      - **AxTableFieldGroupField:**
        - **DataField:** `CreditMax`
      - **AxTableFieldGroupField:**
        - **DataField:** `Blocked`
      - **AxTableFieldGroupField:**
        - **DataField:** `OneTimeCustomer`
      - **AxTableFieldGroupField:**
        - **DataField:** `StatisticsGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountStatement`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForecastDMPInclude`
  - **AxTableFieldGroup:**
    - **Name:** `EPMiniPage`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
  - **AxTableFieldGroup:**
    - **Name:** `EPPayment1`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymTermId`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSpec`
  - **AxTableFieldGroup:**
    - **Name:** `EPPayment2`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSched`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymDayId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CashDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `BankAccount`
  - **AxTableFieldGroup:**
    - **Name:** `EPSetup1`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `InvoiceAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `InvoiceAddress`
      - **AxTableFieldGroupField:**
        - **DataField:** `numberSequenceGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroType`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeFreeTextInvoice`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeInterestNote`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeCollectionletter`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeProjInvoice`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeAccountStatement`
  - **AxTableFieldGroup:**
    - **Name:** `EPSetup2`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `FreightZone`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvTerm`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvReason`
      - **AxTableFieldGroupField:**
        - **DataField:** `DestinationCodeId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesCalendarId`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `EnterpriseNumber`
      - **AxTableFieldGroupField:**
        - **DataField:** `InclTax`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxLicenseNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `FiscalCode`
  - **AxTableFieldGroup:**
    - **Name:** `EPSetup3`
    - **Label:** `@SYS108232`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `RFIDItemTagging`
      - **AxTableFieldGroupField:**
        - **DataField:** `RFIDCaseTagging`
      - **AxTableFieldGroupField:**
        - **DataField:** `RFIDPalletTagging`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterCompanyAutoCreateOrders`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterCompanyDirectDelivery`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterCompanyAllowIndirectCreation`
      - **AxTableFieldGroupField:**
        - **DataField:** `EinvoiceEANNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `PackMaterialFeeLicenseNum`
  - **AxTableFieldGroup:**
    - **Name:** `FederalAttributes`
    - **Label:** `@SPS130`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `FedNonFedIndicator`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustTradingPartnerCode`
      - **AxTableFieldGroupField:**
        - **DataField:** `AgencyLocationCode`
      - **AxTableFieldGroupField:**
        - **DataField:** `IRS1099CIndicator`
      - **AxTableFieldGroupField:**
        - **DataField:** `FederalComments`
  - **AxTableFieldGroup:**
    - **Name:** `FiscalInformation_BR`
    - **Label:** `@GLS56`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustFinalUser_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `ICMSContributor_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `PresenceType_BR`
  - **AxTableFieldGroup:**
    - **Name:** `Gateway`
    - **Label:** `@SYS72001`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `balanceAllCurrency`
  - **AxTableFieldGroup:**
    - **Name:** `GiroMoneyTransferSlip`
    - **Label:** `@SYS316960`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroType`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeFreeTextInvoice`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeInterestNote`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeCollectionletter`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeProjInvoice`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeAccountStatement`
  - **AxTableFieldGroup:**
    - **Name:** `GiroMoneyTransferSlipEP`
    - **Label:** `@SYS316960`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroType`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeFreeTextInvoice`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeInterestNote`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeCollectionletter`
      - **AxTableFieldGroupField:**
        - **DataField:** `GiroTypeProjInvoice`
  - **AxTableFieldGroup:**
    - **Name:** `GovernmentIdentification`
    - **Label:** `@SYS114299`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `IdentificationNumber`
      - **AxTableFieldGroupField:**
        - **DataField:** `PartyCountry`
      - **AxTableFieldGroupField:**
        - **DataField:** `PartyState`
  - **AxTableFieldGroup:**
    - **Name:** `Identification`
    - **Label:** `@SYS5711`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `Party`
      - **AxTableFieldGroupField:**
        - **DataField:** `OrgId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CommercialRegister`
      - **AxTableFieldGroupField:**
        - **DataField:** `CommercialRegisterInsetNumber`
      - **AxTableFieldGroupField:**
        - **DataField:** `CommercialRegisterSection`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForeignResident_RU`
  - **AxTableFieldGroup:**
    - **Name:** `Intercompany`
    - **Label:** `@SYS74106`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `interCompanyTradingRelationActive`
      - **AxTableFieldGroupField:**
        - **DataField:** `interCompanyTradingPartnerCompanyID`
      - **AxTableFieldGroupField:**
        - **DataField:** `interCompanyTradingPartnerAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterCompanyAutoCreateOrders`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterCompanyDirectDelivery`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterCompanyAllowIndirectCreation`
  - **AxTableFieldGroup:**
    - **Name:** `Inventory`
    - **Label:** `@SYS981`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `InventSiteId`
      - **AxTableFieldGroupField:**
        - **DataField:** `InventLocation`
  - **AxTableFieldGroup:**
    - **Name:** `InventoryStatus`
    - **Label:** `@WAX357`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `DefaultInventStatusId`
  - **AxTableFieldGroup:**
    - **Name:** `InventProfile_RU`
    - **Label:** `@GLS113769`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `InventProfileType_RU`
      - **AxTableFieldGroupField:**
        - **DataField:** `InventProfileId_RU`
  - **AxTableFieldGroup:**
    - **Name:** `Invoice`
    - **Label:** `@SYS12128`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `InvoiceAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `InvoiceAddress`
      - **AxTableFieldGroupField:**
        - **DataField:** `numberSequenceGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `UnitedVATInvoice_LT`
      - **AxTableFieldGroupField:**
        - **DataField:** `FiscalDocType_PL`
      - **AxTableFieldGroupField:**
        - **DataField:** `editInvReportFormat`
  - **AxTableFieldGroup:**
    - **Name:** `LeadEditCustomerDetails`
    - **Label:** `@SYS110471`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `customerGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `SegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SubsegmentId`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesDistrictId`
  - **AxTableFieldGroup:**
    - **Name:** `MainContact`
    - **Label:** `@SYS105800`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `MainContactWorker`
  - **AxTableFieldGroup:**
    - **Name:** `MCRCustMerge`
    - **Label:** `@MCR12188`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `MCRMergedRoot`
      - **AxTableFieldGroupField:**
        - **DataField:** `MCRMergedParent`
  - **AxTableFieldGroup:**
    - **Name:** `Memo`
    - **Label:** `@SYS105801`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Memo`
  - **AxTableFieldGroup:**
    - **Name:** `National_BR`
    - **Label:** `@GLS30`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CNPJCPFNum_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `IENum_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `CCMNum_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `NIT_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `INSSCEI_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `CNAE_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForeignerId_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `SimpleNational_BR`
  - **AxTableFieldGroup:**
    - **Name:** `NotificationToTheCentralBank`
    - **Label:** `@SYS67156`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `BankCentralBankPurposeCode`
      - **AxTableFieldGroupField:**
        - **DataField:** `BankCentralBankPurposeText`
  - **AxTableFieldGroup:**
    - **Name:** `PackagingMaterialFee`
    - **Label:** `@SYS72996`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PackMaterialFeeLicenseNum`
  - **AxTableFieldGroup:**
    - **Name:** `Payment`
    - **Label:** `@SYS828`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymTermId`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSpec`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymSched`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymDayId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CashDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `CashDiscBaseDays`
      - **AxTableFieldGroupField:**
        - **DataField:** `BankAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `FactoringAccount`
      - **AxTableFieldGroupField:**
        - **DataField:** `BankCustPaymIdTable`
      - **AxTableFieldGroupField:**
        - **DataField:** `bankAccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `UseCashDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymentReference_EE`
      - **AxTableFieldGroupField:**
        - **DataField:** `IntBank_LV`
      - **AxTableFieldGroupField:**
        - **DataField:** `LvPaymTransCodes`
      - **AxTableFieldGroupField:**
        - **DataField:** `FineCode_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `InterestCode_BR`
  - **AxTableFieldGroup:**
    - **Name:** `PdsFreightAccrued`
    - **Label:** `@SYS1655`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PdsFreightAccrued`
  - **AxTableFieldGroup:**
    - **Name:** `Posting`
    - **Label:** `@SYS12919`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
  - **AxTableFieldGroup:**
    - **Name:** `quickCreateDetails`
    - **Label:** `@SYS340108`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
      - **AxTableFieldGroupField:**
        - **DataField:** `PaymTermId`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvTerm`
      - **AxTableFieldGroupField:**
        - **DataField:** `DlvMode`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `OrgId`
  - **AxTableFieldGroup:**
    - **Name:** `quickCreateHeader`
    - **Label:** `@SYS340399`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `AccountNum`
  - **AxTableFieldGroup:**
    - **Name:** `ReturnablePackage_PL`
    - **Label:** `@GLS110272`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PackageDepositExcempt_PL`
  - **AxTableFieldGroup:**
    - **Name:** `SalesDiscount`
    - **Label:** `@SYS14375`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `MultiLineDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `EndDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `PriceGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `LineDisc`
      - **AxTableFieldGroupField:**
        - **DataField:** `PdsCustRebateGroupId`
      - **AxTableFieldGroupField:**
        - **DataField:** `PdsRebateTMAGroup`
  - **AxTableFieldGroup:**
    - **Name:** `SalesDistrict`
    - **Label:** `@SYS105802`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesDistrictId`
  - **AxTableFieldGroup:**
    - **Name:** `SalesOrder`
    - **Label:** `@SYS7443`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `MarkupGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `InventSiteId`
      - **AxTableFieldGroupField:**
        - **DataField:** `InventLocation`
      - **AxTableFieldGroupField:**
        - **DataField:** `CustItemGroupId`
      - **AxTableFieldGroupField:**
        - **DataField:** `CommissionGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `SalesPoolId`
      - **AxTableFieldGroupField:**
        - **DataField:** `OurAccountNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `interCompanyTradingPartnerCompanyID`
      - **AxTableFieldGroupField:**
        - **DataField:** `OrderEntryDeadlineGroupId`
      - **AxTableFieldGroupField:**
        - **DataField:** `InvoicePostingType_RU`
      - **AxTableFieldGroupField:**
        - **DataField:** `ExportSales_PL`
  - **AxTableFieldGroup:**
    - **Name:** `SalesOrderOther`
    - **Label:** `@SYS30289`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SuppItemGroupId`
  - **AxTableFieldGroup:**
    - **Name:** `SalesReturn_BR`
    - **Label:** `@GLS220586`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `GenerateIncomingFiscalDocument_BR`
  - **AxTableFieldGroup:**
    - **Name:** `SalesTax`
    - **Label:** `@SYS5878`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `EnterpriseNumber`
      - **AxTableFieldGroupField:**
        - **DataField:** `InclTax`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxLicenseNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `FiscalCode`
      - **AxTableFieldGroupField:**
        - **DataField:** `MandatoryVatDate_PL`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxPeriodPaymentCode_PL`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxGSTReliefGroupHeading_MY`
      - **AxTableFieldGroupField:**
        - **DataField:** `UseOriginalDocumentAsFacture_RU`
      - **AxTableFieldGroupField:**
        - **DataField:** `OverrideSalesTax`
  - **AxTableFieldGroup:**
    - **Name:** `SalesTaxEP`
    - **Label:** `@SYS5878`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `InclTax`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxLicenseNum`
  - **AxTableFieldGroup:**
    - **Name:** `SalesTaxExchangeRate`
    - **Label:** `@GLS112406`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PassportNo_HU`
      - **AxTableFieldGroupField:**
        - **DataField:** `IssuerCountry_HU`
  - **AxTableFieldGroup:**
    - **Name:** `Segment`
    - **Label:** `@SYS105803`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SegmentId`
  - **AxTableFieldGroup:**
    - **Name:** `Services_BR`
    - **Label:** `@SYS35620`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ServiceCodeOnDlvAddress_BR`
  - **AxTableFieldGroup:**
    - **Name:** `Subsegment`
    - **Label:** `@SYS105805`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SubsegmentId`
  - **AxTableFieldGroup:**
    - **Name:** `SuframaRegion_BR`
    - **Label:** `@GLS129`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Suframa_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `SuframaNumber_BR`
      - **AxTableFieldGroupField:**
        - **DataField:** `SuframaPISCOFINS_BR`
  - **AxTableFieldGroup:**
    - **Name:** `SupplementaryItem`
    - **Label:** `@SYS58240`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SuppItemGroupId`
  - **AxTableFieldGroup:**
    - **Name:** `TaxRegistration_MX`
    - **Label:** `@SYS312490`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyType_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `Rfc_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `Curp_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `StateInscription_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForeignTaxRegistration_MX`
  - **AxTableFieldGroup:**
    - **Name:** `TMS`
    - **Label:** `@TRX1`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `ExpressBillOfLading`
  - **AxTableFieldGroup:**
    - **Name:** `Vendor`
    - **Label:** `@SYS9455`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `VendAccount`
  - **AxTableFieldGroup:**
    - **Name:** `WebCategoryBrowsing`
    - **Label:** `@SYS74258`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustGroup`
      - **AxTableFieldGroupField:**
        - **DataField:** `Currency`
  - **AxTableFieldGroup:**
    - **Name:** `WhtContribution_BR`
    - **Label:** `@FBK30`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `CustWhtContributionType_BR`
  - **AxTableFieldGroup:**
    - **Name:** `WithholdingTax`
    - **Label:** `@SYS7372`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxWithholdCalculate_TH`
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxWithholdGroup_TH`
  - **AxTableFieldGroup:**
    - **Name:** `WithholdingTax_IN`
    - **Label:** `@SYS33817`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `TaxWithholdCalculate_IN`
  - **AxTableFieldGroup:**
    - **Name:** `MexicanSharedInfoByParty`
    - **Label:** `@Mexico:DirPartyInformationCopy_TableGroup`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `Rfc_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `CompanyType_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `Curp_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `StateInscription_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForeignTaxRegistration_MX`
  - **AxTableFieldGroup:**
    - **Name:** `EInvoice_MX`
    - **Label:** `@SYS341123`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SATPaymMethod_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `SATPurpose_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `CFDIEnabled_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `ForeignTrade_MX`
      - **AxTableFieldGroupField:**
        - **DataField:** `CFDISkipIEPSTaxes_MX`
  - **AxTableFieldGroup:**
    - **Name:** `WorkflowState`
    - **Label:** `@SYS121130`
    - **Fields:**
  - **AxTableFieldGroup:**
    - **Name:** `TaxIntgrExport_CN`
    - **Label:** `@TaxIntgr:TaxRegistrationAndBankAcc`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `SimplifyTaxIntgrExportDocValidation_CN`
  - **AxTableFieldGroup:**
    - **Name:** `VATNum`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNum`
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNumRecId`
      - **AxTableFieldGroupField:**
        - **DataField:** `VATNumTableType`
  - **AxTableFieldGroup:**
    - **Name:** `PrepaymentConfig`
    - **Label:** `@AccountsReceivable:Prepayment`
    - **Fields:**
      - **AxTableFieldGroupField:**
        - **DataField:** `PrepayType`
      - **AxTableFieldGroupField:**
        - **DataField:** `PrepaymentValue`
- **Fields:**
  - **AxTableField:**
    - **Name:** `PaymTermId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustPaymTermId`
  - **AxTableField:**
    - **Name:** `LineDisc`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustLineDiscCode`
    - **Label:** `@SYS316407`
  - **AxTableField:**
    - **Name:** `TaxWithholdGroup_TH`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TaxWithholdGroup`
    - **FeatureClass:** `TaxWithholdingGlobalItemGroupToggle`
  - **AxTableField:**
    - **Name:** `PartyCountry`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `AddressCountryRegionId`
  - **AxTableField:**
    - **Name:** `AccountNum`
    - **AllowEdit:** `No`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustAccount`
    - **Mandatory:** `Yes`
  - **AxTableField:**
    - **Name:** `AccountStatement`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `CustAccountStatement`
  - **AxTableField:**
    - **Name:** `Affiliated_RU`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustAffiliated_RU`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `AgencyLocationCode`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `AgencyLocationCode`
  - **AxTableField:**
    - **Name:** `BankAccount`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustBankAccountId`
  - **AxTableField:**
    - **Name:** `BankCentralBankPurposeCode`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `BankCentralBankPurposeCode`
  - **AxTableField:**
    - **Name:** `BankCentralBankPurposeText`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `BankCentralBankPurposeText`
    - **Label:** `@SYS316424`
  - **AxTableField:**
    - **Name:** `BankCustPaymIdTable`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `BankCustPaymIdRecId`
    - **FeatureClass:** `CustConfigurablePaymentIdToggle_CH`
  - **AxTableField:**
    - **Name:** `BirthCountyCode_IT`
    - **AosAuthorization:** `Yes`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `AddressCountyCode_IT`
    - **Label:** `@SYS343696`
  - **AxTableField:**
    - **Name:** `BirthPlace_IT`
    - **AosAuthorization:** `Yes`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `IT`
    - **ExtendedDataType:** `AddressCity`
    - **Label:** `@SYS81761`
  - **AxTableField:**
    - **Name:** `Blocked`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustBlocked`
    - **EnumType:** `CustVendorBlocked`
  - **AxTableField:**
    - **Name:** `CashDisc`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustCashDiscCode`
  - **AxTableField:**
    - **Name:** `CashDiscBaseDays`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CashDiscBaseDays`
  - **AxTableField:**
    - **Name:** `CCMNum_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CCMNum_BR`
  - **AxTableField:**
    - **Name:** `ClearingPeriod`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustClearingPeriod`
  - **AxTableField:**
    - **Name:** `CNAE_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CNAE_BR`
  - **AxTableField:**
    - **Name:** `CNPJCPFNum_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CNPJCPFNum_BR`
  - **AxTableField:**
    - **Name:** `CommercialRegister`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CommercialRegister`
    - **IsObsolete:** `Yes`
  - **AxTableField:**
    - **Name:** `CommercialRegisterInsetNumber`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CommercialRegisterInsetNumber`
    - **IsObsolete:** `Yes`
  - **AxTableField:**
    - **Name:** `CommercialRegisterSection`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CommercialRegisterSection`
    - **IsObsolete:** `Yes`
  - **AxTableField:**
    - **Name:** `CommissionGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CommissCustomerGroup`
  - **AxTableField:**
    - **Name:** `CompanyChainId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `smmChainId`
    - **Label:** `@SYS316639`
  - **AxTableField:**
    - **Name:** `CompanyIdSiret`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CompanyIdSiret`
  - **AxTableField:**
    - **Name:** `CompanyNAFCode`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `CompanyNAFRecId`
  - **AxTableField:**
    - **Name:** `CompanyType_MX`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `CompanyType_MX`
  - **AxTableField:**
    - **Name:** `ConsDay_JP`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustConsDay_JP`
  - **AxTableField:**
    - **Name:** `ContactPersonId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `ContactPersonId`
    - **Label:** `@SYS316632`
  - **AxTableField:**
    - **Name:** `CreditCardAddressVerification`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `CreditCardAddressVerification`
  - **AxTableField:**
    - **Name:** `CreditCardAddressVerificationLevel`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `CreditCardAddressVerificationLevel`
  - **AxTableField:**
    - **Name:** `CreditCardAddressVerificationVoid`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CreditCardAddressVerificationVoid`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `CreditCardCVC`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `CreditCardCVC`
  - **AxTableField:**
    - **Name:** `CreditMax`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustCreditMaxMST`
  - **AxTableField:**
    - **Name:** `CreditRating`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustCreditRating`
  - **AxTableField:**
    - **Name:** `Curp_MX`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `Curp_MX`
  - **AxTableField:**
    - **Name:** `Currency`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustCurrencyCode`
    - **Mandatory:** `Yes`
  - **AxTableField:**
    - **Name:** `CustClassificationId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustClassificationId`
  - **AxTableField:**
    - **Name:** `CustExcludeCollectionFee`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustExcludeCollectionFee`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `CustExcludeInterestCharges`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustExcludeInterestCharges`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `CustFinalUser_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustVendFinalUser_BR`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `CustGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustGroupId`
    - **Mandatory:** `Yes`
  - **AxTableField:**
    - **Name:** `CustItemGroupId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustItemGroupId`
  - **AxTableField:**
    - **Name:** `CustTradingPartnerCode`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `TradingPartnerCodeRecId`
  - **AxTableField:**
    - **Name:** `CustWhtContributionType_BR`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `BR`
    - **ExtendedDataType:** `WhtContributionType_BR`
    - **EnumType:** `CustWhtContributionType_BR`
  - **AxTableField:**
    - **Name:** `DefaultDimension`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `LedgerDefaultDimensionValueSet`
    - **SysSharingType:** `Never`
  - **AxTableField:**
    - **Name:** `DefaultDirectDebitMandate`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `CustDirectDebitMandateRecId`
  - **AxTableField:**
    - **Name:** `DefaultInventStatusId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `WHSDefaultStatusId`
  - **AxTableField:**
    - **Name:** `DestinationCodeId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustDestinationCodeId`
  - **AxTableField:**
    - **Name:** `DlvMode`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustDlvModeId`
  - **AxTableField:**
    - **Name:** `DlvReason`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `DlvReasonId`
  - **AxTableField:**
    - **Name:** `DlvTerm`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustDlvTermId`
  - **AxTableField:**
    - **Name:** `EInvoice`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `NoYesId`
    - **Label:** `@SYS344739`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `EInvoiceAttachment`
    - **ExtendedDataType:** `NoYesId`
    - **HelpText:** `@AccountsReceivable:EInvoiceAttachmentHelp`
    - **Label:** `@AccountsReceivable:EInvoiceAttachment`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `EinvoiceEANNum`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `DK`
    - **ExtendedDataType:** `EinvoiceEANNum`
  - **AxTableField:**
    - **Name:** `EndDisc`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustEndDiscCode`
    - **Label:** `@SYS316403`
  - **AxTableField:**
    - **Name:** `EnterpriseNumber`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SysDeletedObjects72`
    - **ExtendedDataType:** `TaxEnterpriseNumber`
    - **IsObsolete:** `Yes`
  - **AxTableField:**
    - **Name:** `EntryCertificateRequired_W`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `EntryCertificateRequired_W`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ExportSales_PL`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PlExportSales`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ExpressBillOfLading`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TMSExpressBillofLading`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `FactoringAccount`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `BankCustFactoringAccount`
  - **AxTableField:**
    - **Name:** `FederalComments`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `FederalComments`
  - **AxTableField:**
    - **Name:** `FedNonFedIndicator`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `FederalNonFederalIndicatorCode`
  - **AxTableField:**
    - **Name:** `FineCode_BR`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `BR`
    - **ExtendedDataType:** `CustFineCode_BR`
  - **AxTableField:**
    - **Name:** `FiscalCode`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `FiscalCode`
  - **AxTableField:**
    - **Name:** `FiscalDocType_PL`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `PL`
    - **EnumType:** `PlFiscalDocType`
  - **AxTableField:**
    - **Name:** `ForecastDMPInclude`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `ForecastDMPInclude`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ForeignResident_RU`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `ForeignResident_RU`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `FreightZone`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustFreightZoneId`
  - **AxTableField:**
    - **Name:** `GenerateIncomingFiscalDocument_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `GenerateIncomingFiscalDocument_BR`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `GiroType`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymentStubInvoiceId`
    - **Label:** `@SYS316648`
    - **EnumType:** `PaymentStub`
  - **AxTableField:**
    - **Name:** `GiroTypeAccountStatement`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymentStubAccountStatementId`
    - **EnumType:** `PaymentStub`
  - **AxTableField:**
    - **Name:** `GiroTypeCollectionletter`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymentStubCollectionId`
    - **Label:** `@SYS316654`
    - **EnumType:** `PaymentStub`
  - **AxTableField:**
    - **Name:** `GiroTypeFreeTextInvoice`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymentStubInvoiceId`
    - **Label:** `@SYS316650`
    - **EnumType:** `PaymentStub`
  - **AxTableField:**
    - **Name:** `GiroTypeInterestNote`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymentStubInterestId`
    - **Label:** `@SYS316652`
    - **EnumType:** `PaymentStub`
  - **AxTableField:**
    - **Name:** `GiroTypeProjInvoice`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymentStubProjId`
    - **Label:** `@SYS316656`
    - **EnumType:** `PaymentStub`
  - **AxTableField:**
    - **Name:** `ICMSContributor_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `ICMSContributor_BR`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `IdentificationNumber`
    - **AosAuthorization:** `Yes`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustIdentificationNumber`
  - **AxTableField:**
    - **Name:** `IENum_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `IENum_BR`
  - **AxTableField:**
    - **Name:** `InclTax`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InclTax`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `INSSCEI_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `INSSCEI_BR`
  - **AxTableField:**
    - **Name:** `IntBank_LV`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `LV`
    - **ExtendedDataType:** `CustBankAccountId`
    - **Label:** `@GLS108468`
  - **AxTableField:**
    - **Name:** `InterCompanyAllowIndirectCreation`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InterCompanyAllowIndirectCreation`
    - **Label:** `@SYS316385`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `InterCompanyAutoCreateOrders`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InterCompanyAutoCreateOrders`
    - **Label:** `@SYS316382`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `InterCompanyDirectDelivery`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InterCompanyDirectDelivery`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `InterestCode_BR`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `BR`
    - **ExtendedDataType:** `CustInterestCode_BR`
  - **AxTableField:**
    - **Name:** `InventLocation`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InventLocationId`
    - **SysSharingType:** `Optional`
  - **AxTableField:**
    - **Name:** `InventProfileId_RU`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InventProfileId_RU`
  - **AxTableField:**
    - **Name:** `InventProfileType_RU`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `InventProfileType_RU`
  - **AxTableField:**
    - **Name:** `InventSiteId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InventSiteId`
    - **SysSharingType:** `Optional`
  - **AxTableField:**
    - **Name:** `InvoiceAccount`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustInvoiceAccount`
  - **AxTableField:**
    - **Name:** `InvoiceAddress`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustInvoiceAddress`
    - **EnumType:** `InvoiceOrderAccount`
  - **AxTableField:**
    - **Name:** `InvoicePostingType_RU`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `SalesInvoicePostingType_RU`
  - **AxTableField:**
    - **Name:** `IRS1099CIndicator`
    - **AosAuthorization:** `Yes`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustIRS1099CIndicator`
    - **Label:** `@SPS136`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `IsResident_LV`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `LvResident`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `IssueOwnEntryCertificate_W`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `IssueOwnEntryCertificate_W`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `IssuerCountry_HU`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `HU`
    - **ExtendedDataType:** `AddressCountryRegionId`
    - **Label:** `@GLS112618`
  - **AxTableField:**
    - **Name:** `LineOfBusinessId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustLineOfBusinessId`
  - **AxTableField:**
    - **Name:** `LvPaymTransCodes`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `PaymTransCodeRecId`
  - **AxTableField:**
    - **Name:** `MainContactWorker`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `smmBusRelAccResponsibleWorker`
    - **Label:** `@SYS316635`
  - **AxTableField:**
    - **Name:** `MandatoryCreditLimit`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `MandatoryCreditLimit`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `MandatoryVatDate_PL`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PlMandatoryVatDate`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `MarkupGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustMarkupGroupId`
  - **AxTableField:**
    - **Name:** `MCRMergedParent`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustAccount`
    - **IgnoreEDTRelation:** `Yes`
    - **Label:** `@MCR12159`
  - **AxTableField:**
    - **Name:** `MCRMergedRoot`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustAccount`
    - **IgnoreEDTRelation:** `Yes`
    - **Label:** `@MCR12158`
  - **AxTableField:**
    - **Name:** `Memo`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SmmCRM`
    - **ExtendedDataType:** `smmBusRelMemo`
  - **AxTableField:**
    - **Name:** `MultiLineDisc`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustMultiLineDiscCode`
    - **Label:** `@SYS316401`
  - **AxTableField:**
    - **Name:** `NIT_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `NIT_BR`
  - **AxTableField:**
    - **Name:** `numberSequenceGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `NumberSequenceGroupId`
  - **AxTableField:**
    - **Name:** `OneTimeCustomer`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `OneTimeCustomer`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `OrderEntryDeadlineGroupId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InventOrderEntryDeadlineGroupId`
    - **Label:** `@SYS111450`
  - **AxTableField:**
    - **Name:** `OrgId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `OrgId`
  - **AxTableField:**
    - **Name:** `OurAccountNum`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustAccountExt`
    - **Label:** `@SYS316394`
  - **AxTableField:**
    - **Name:** `PackageDepositExcempt_PL`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PlPackageDepositExcempt`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `PackedExtensions`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `SysExtensionSerializerPackedContainer`
    - **SaveContents:** `No`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `PackMaterialFeeLicenseNum`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `InventPackingMaterialFeeLicenseNum`
    - **Label:** `@SYS316376`
  - **AxTableField:**
    - **Name:** `Party`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `DirPartyRecId`
  - **AxTableField:**
    - **Name:** `PartyState`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `LogisticsAddressStateId`
  - **AxTableField:**
    - **Name:** `PassportNo_HU`
    - **AosAuthorization:** `Yes`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `HU`
    - **ExtendedDataType:** `PassportNo_HU`
  - **AxTableField:**
    - **Name:** `PaymDayId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustPaymDayId`
  - **AxTableField:**
    - **Name:** `PaymentReference_EE`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `EE`
    - **ExtendedDataType:** `CustPaymReference_EE`
  - **AxTableField:**
    - **Name:** `PaymIdType`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `BankCustPaymIdType`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `PaymMode`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustPaymMode`
  - **AxTableField:**
    - **Name:** `PaymSched`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymSchedId`
  - **AxTableField:**
    - **Name:** `PaymSpec`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PaymSpec`
  - **AxTableField:**
    - **Name:** `PdsCustRebateGroupId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PdsCustRebateGroupId`
  - **AxTableField:**
    - **Name:** `PdsFreightAccrued`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PdsFreightAccrued`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `PdsRebateTMAGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PdsRebateProgramTMAGroup`
  - **AxTableField:**
    - **Name:** `PriceGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustPriceGroup`
  - **AxTableField:**
    - **Name:** `ResidenceForeignCountryRegionId_IT`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `IT`
    - **ExtendedDataType:** `LogisticsAddressCountryRegionId`
    - **Label:** `@SYS81763`
  - **AxTableField:**
    - **Name:** `Rfc_MX`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `Rfc_MX`
  - **AxTableField:**
    - **Name:** `RFIDCaseTagging`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `RFIDCaseTagging`
    - **IsObsolete:** `Yes`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `RFIDItemTagging`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `RFIDItemTagging`
    - **IsObsolete:** `Yes`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `RFIDPalletTagging`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `RFIDPalletTagging`
    - **IsObsolete:** `Yes`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `SalesCalendarId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `SalesCalendarId`
  - **AxTableField:**
    - **Name:** `SalesDistrictId`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SmmCRM`
    - **ExtendedDataType:** `smmSalesDistrictId`
  - **AxTableField:**
    - **Name:** `SalesGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CommissSalesGroup`
  - **AxTableField:**
    - **Name:** `SalesPoolId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustSalesPoolId`
  - **AxTableField:**
    - **Name:** `SegmentId`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SmmCRM`
    - **ExtendedDataType:** `smmSegmentId`
  - **AxTableField:**
    - **Name:** `ServiceCodeOnDlvAddress_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TaxServiceCodeOnDlvAddress_BR`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ShipCarrierAccount`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `ShipCarrier`
    - **ExtendedDataType:** `ShipCarrierAccount`
  - **AxTableField:**
    - **Name:** `ShipCarrierAccountCode`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `ShipCarrier`
    - **ExtendedDataType:** `ShipCarrierAccountCode`
  - **AxTableField:**
    - **Name:** `ShipCarrierBlindShipment`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `ShipCarrier`
    - **ExtendedDataType:** `ShipCarrierBlindShipment`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ShipCarrierFuelSurcharge`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `ShipCarrier`
    - **ExtendedDataType:** `ShipCarrierFuelSurcharge`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ShipCarrierId`
    - **AllowEdit:** `No`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `ShipCarrier`
    - **ExtendedDataType:** `ShipCarrierId`
  - **AxTableField:**
    - **Name:** `StateInscription_MX`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `StateInscription_MX`
  - **AxTableField:**
    - **Name:** `StatisticsGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustStatGroupId`
  - **AxTableField:**
    - **Name:** `SubsegmentId`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SmmCRM`
    - **ExtendedDataType:** `smmSubsegmentId`
  - **AxTableField:**
    - **Name:** `Suframa_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustSuframa_BR`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `SuframaNumber_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustSuframaNumber_BR`
  - **AxTableField:**
    - **Name:** `SuframaPISCOFINS_BR`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `BR`
    - **Label:** `@GLS1335`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `SuppItemGroupId`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `CustSuppItemGroupId`
  - **AxTableField:**
    - **Name:** `TaxGroup`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TaxGroup`
  - **AxTableField:**
    - **Name:** `TaxLicenseNum`
    - **AosAuthorization:** `Yes`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TaxPackagingLicenseNum`
  - **AxTableField:**
    - **Name:** `TaxPeriodPaymentCode_PL`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `PlTaxPeriodPaymentCode`
  - **AxTableField:**
    - **Name:** `TaxWithholdCalculate_IN`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `IN`
    - **ExtendedDataType:** `NoYesId`
    - **Label:** `@SYS81757`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `TaxWithholdCalculate_TH`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TaxWithholdCalculate_TH`
    - **FeatureClass:** `TaxWithholdingGlobalItemGroupToggle`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `UnitedVATInvoice_LT`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `UnitedVATInvoice_LT`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `UseCashDisc`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `UseCashDisc`
  - **AxTableField:**
    - **Name:** `usePurchRequest`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `RetailUsePurchRequest`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `VATNum`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `VATNum`
  - **AxTableField:**
    - **Name:** `VendAccount`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `VendAccount`
  - **AxTableField:**
    - **Name:** `WebSalesOrderDisplay`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `ECPsalesOrdersViewType`
  - **AxTableField:**
    - **Name:** `AuthorityOffice_IT`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `AuthorityOffice_IT`
  - **AxTableField:**
    - **Name:** `EInvoiceRegister_IT`
    - **AssetClassification:** `Customer Content`
    - **CountryRegionCodes:** `IT,ES`
    - **ExtendedDataType:** `NoYesId`
    - **Label:** `@SYP4881672`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ForeignerId_BR`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `EFDocForeignerId_BR`
  - **AxTableField:**
    - **Name:** `PresenceType_BR`
    - **AssetClassification:** `Customer Content`
    - **EnumType:** `EFDocPresenceType_BR`
  - **AxTableField:**
    - **Name:** `TaxGSTReliefGroupHeading_MY`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `TaxGSTReliefGroupRecId_MY`
  - **AxTableField:**
    - **Name:** `ForeignTaxRegistration_MX`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `ForeignTaxRegistration_MX`
  - **AxTableField:**
    - **Name:** `CustWriteOffRefRecId`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `RefRecId`
    - **SysSharingType:** `Never`
  - **AxTableField:**
    - **Name:** `RegNum_W`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SysDeletedObjects72`
    - **CountryRegionCodes:** `RU,EE,LV,LT,HU,PL,CZ`
    - **ExtendedDataType:** `CompanyRegNum`
    - **HelpText:** `@SYS24188`
    - **Label:** `@GLS108954`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `EnterpriseCode`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SysDeletedObjects72`
    - **ExtendedDataType:** `EnterpriseCode`
    - **Label:** `@gls220837`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `TaxBorderNumber_FI`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SysDeletedObjects72`
    - **ExtendedDataType:** `TaxBorderNumber_FI`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `BirthDate_IT`
    - **AssetClassification:** `Customer Content`
    - **ConfigurationKey:** `SYSDeletedObjects72`
    - **CountryRegionCodes:** `IT`
    - **ExtendedDataType:** `BirthDate`
    - **Label:** `@SYS343698`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `IsExternallyMaintained`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `IsExternallyMaintained`
    - **Label:** `@AccountsReceivable:IsExternallyMaintained`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `SATPaymMethod_MX`
    - **ExtendedDataType:** `SATPaymMethod_MX`
  - **AxTableField:**
    - **Name:** `SATPurpose_MX`
    - **ExtendedDataType:** `SATPurpose_MX`
  - **AxTableField:**
    - **Name:** `CFDIEnabled_MX`
    - **ExtendedDataType:** `CFDIPackingSlipEnabled_MX`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `ForeignTrade_MX`
    - **ExtendedDataType:** `ForeignTrade_MX`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `WorkflowState`
    - **EnumType:** `CustTableChangeProposalWorkflowState`
  - **AxTableField:**
    - **Name:** `UseOriginalDocumentAsFacture_RU`
    - **ExtendedDataType:** `UseOriginalDocumentAsFacture_RU`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `CollectionLetterCode`
    - **AssetClassification:** `Customer Content`
    - **HelpText:** `@SYS75417`
    - **Label:** `@SYS75418`
    - **EnumType:** `CustCollectionLetterCode`
  - **AxTableField:**
    - **Name:** `BlockFloorLimitUseInChannel`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `BlockFloorLimitUseInChannel`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `CFDISkipIEPSTaxes_MX`
    - **CountryRegionCodes:** `MX`
    - **ExtendedDataType:** `CFDISkipIEPS_MX`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `SimplifyTaxIntgrExportDocValidation_CN`
    - **CountryRegionCodes:** `CN`
    - **HelpText:** `@TaxIntgr:HelpTextForDisablingValidation`
    - **Label:** `@TaxIntgr:DisableValidation`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `SimpleNational_BR`
    - **ExtendedDataType:** `FBSpedSimpleNational_BR`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `VATNumRecId`
    - **AssetClassification:** `System Metadata`
    - **ExtendedDataType:** `RefRecId`
    - **Visible:** `No`
  - **AxTableField:**
    - **Name:** `VATNumTableType`
    - **AssetClassification:** `System Metadata`
    - **Visible:** `No`
    - **EnumType:** `TaxExemptNumberSourceType`
  - **AxTableField:**
    - **Name:** `OverrideSalesTax`
    - **AssetClassification:** `Customer Content`
    - **ExtendedDataType:** `TaxIntegrationOverrideSalesTax`
    - **Label:** `@TaxIntegration:OverrideSalesTax`
    - **EnumType:** `NoYes`
  - **AxTableField:**
    - **Name:** `PrepayType`
    - **AssetClassification:** `Customer Content`
    - **Label:** `@AccountsReceivable:PrepaymentType`
    - **EnumType:** `SalesPrepayType`
  - **AxTableField:**
    - **Name:** `PrepaymentValue`
    - **ExtendedDataType:** `AmountCur`
    - **Label:** `@AccountsReceivable:PrepaymentValue`
- **FullTextIndexes:**
- **Indexes:**
  - **AxTableIndex:**
    - **Name:** `AccountIdx`
    - **AllowPageLocks:** `No`
    - **AlternateKey:** `Yes`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `AccountNum`
  - **AxTableIndex:**
    - **Name:** `Party`
    - **AllowPageLocks:** `No`
    - **AlternateKey:** `Yes`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `Party`
      - **AxTableIndexField:**
        - **DataField:** `dataAreaId`
  - **AxTableIndex:**
    - **Name:** `InvoiceAccountIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `InvoiceAccount`
      - **AxTableIndexField:**
        - **DataField:** `Partition`
      - **AxTableIndexField:**
        - **DataField:** `DataAreaId`
  - **AxTableIndex:**
    - **Name:** `OurAccountNumIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `OurAccountNum`
  - **AxTableIndex:**
    - **Name:** `CompanyNAFCodeIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `CompanyNAFCode`
  - **AxTableIndex:**
    - **Name:** `BankCustPaymIdTableIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `BankCustPaymIdTable`
  - **AxTableIndex:**
    - **Name:** `HcmWorkerIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `MainContactWorker`
  - **AxTableIndex:**
    - **Name:** `PaymRefIdx_EE`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `PaymentReference_EE`
  - **AxTableIndex:**
    - **Name:** `LogisticsAddressCountryRegionIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `ResidenceForeignCountryRegionId_IT`
  - **AxTableIndex:**
    - **Name:** `PlTaxDueTableIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `TaxPeriodPaymentCode_PL`
  - **AxTableIndex:**
    - **Name:** `CustTradingPartnerCodeIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **ConfigurationKey:** `PublicSector`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `CustTradingPartnerCode`
  - **AxTableIndex:**
    - **Name:** `LvPaymTransCodesIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `LvPaymTransCodes`
  - **AxTableIndex:**
    - **Name:** `VendAccount`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `VendAccount`
  - **AxTableIndex:**
    - **Name:** `MCRMergeIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **ConfigurationKey:** `MCRCallCenter`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `MCRMergedRoot`
  - **AxTableIndex:**
    - **Name:** `CustGroupIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `CustGroup`
  - **AxTableIndex:**
    - **Name:** `CustWriteOffFinancialReasonsSetupIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `CustWriteOffRefRecId`
  - **AxTableIndex:**
    - **Name:** `CNPJCPFNUM_BRIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `CNPJCPFNUM_BR`
  - **AxTableIndex:**
    - **Name:** `FOREIGNERID_BRIdx`
    - **AllowDuplicates:** `Yes`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `FOREIGNERID_BR`
  - **AxTableIndex:**
    - **Name:** `AccountNumInclRecIdIdx`
    - **AllowPageLocks:** `No`
    - **Fields:**
      - **AxTableIndexField:**
        - **DataField:** `AccountNum`
      - **AxTableIndexField:**
        - **DataField:** `RecId`
        - **IncludedColumn:** `Yes`
- **Mappings:**
  - **AxTableMapping:**
    - **MappingTable:** `AifEndpointConstraintMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `ConstraintId`
        - **MapFieldTo:** `AccountNum`
  - **AxTableMapping:**
    - **MappingTable:** `CustVendAccountMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `Account`
        - **MapFieldTo:** `AccountNum`
      - **AxTableMappingConnection:**
        - **MapField:** `Num`
        - **MapFieldTo:** `AccountNum`
  - **AxTableMapping:**
    - **MappingTable:** `CustVendTable`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `AccountNum`
        - **MapFieldTo:** `AccountNum`
      - **AxTableMappingConnection:**
        - **MapField:** `BankAccountId`
        - **MapFieldTo:** `BankAccount`
      - **AxTableMappingConnection:**
        - **MapField:** `BankCentralBankPurposeCode`
        - **MapFieldTo:** `BankCentralBankPurposeCode`
      - **AxTableMappingConnection:**
        - **MapField:** `BankCentralBankPurposeText`
        - **MapFieldTo:** `BankCentralBankPurposeText`
      - **AxTableMappingConnection:**
        - **MapField:** `CompanyType_MX`
        - **MapFieldTo:** `CompanyType_MX`
      - **AxTableMappingConnection:**
        - **MapField:** `ConsDay_JP`
        - **MapFieldTo:** `ConsDay_JP`
      - **AxTableMappingConnection:**
        - **MapField:** `ContactPersonId`
        - **MapFieldTo:** `ContactPersonId`
      - **AxTableMappingConnection:**
        - **MapField:** `CreditMax`
        - **MapFieldTo:** `CreditMax`
      - **AxTableMappingConnection:**
        - **MapField:** `Currency`
        - **MapFieldTo:** `Currency`
      - **AxTableMappingConnection:**
        - **MapField:** `CustCollectionsContactPersonId`
      - **AxTableMappingConnection:**
        - **MapField:** `CustVendAccount`
        - **MapFieldTo:** `VendAccount`
      - **AxTableMappingConnection:**
        - **MapField:** `CustVendAccountExt`
        - **MapFieldTo:** `OurAccountNum`
      - **AxTableMappingConnection:**
        - **MapField:** `CustVendCNPJCPF_BR`
        - **MapFieldTo:** `CNPJCPFNum_BR`
      - **AxTableMappingConnection:**
        - **MapField:** `CustVendIE_BR`
        - **MapFieldTo:** `IENum_BR`
      - **AxTableMappingConnection:**
        - **MapField:** `DefaultDimension`
        - **MapFieldTo:** `DefaultDimension`
      - **AxTableMappingConnection:**
        - **MapField:** `FactoringAccount`
        - **MapFieldTo:** `FactoringAccount`
      - **AxTableMappingConnection:**
        - **MapField:** `FiscalCode`
        - **MapFieldTo:** `FiscalCode`
      - **AxTableMappingConnection:**
        - **MapField:** `ForeignTaxRegistration_MX`
        - **MapFieldTo:** `ForeignTaxRegistration_MX`
      - **AxTableMappingConnection:**
        - **MapField:** `GroupId`
        - **MapFieldTo:** `CustGroup`
      - **AxTableMappingConnection:**
        - **MapField:** `InvoiceAccount`
        - **MapFieldTo:** `InvoiceAccount`
      - **AxTableMappingConnection:**
        - **MapField:** `LineOfBusinessId`
        - **MapFieldTo:** `LineOfBusinessId`
      - **AxTableMappingConnection:**
        - **MapField:** `OrgId`
        - **MapFieldTo:** `OrgId`
      - **AxTableMappingConnection:**
        - **MapField:** `Party`
        - **MapFieldTo:** `Party`
      - **AxTableMappingConnection:**
        - **MapField:** `PartyType`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymDayId`
        - **MapFieldTo:** `PaymDayId`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymId`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymMode`
        - **MapFieldTo:** `PaymMode`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymSchedId`
        - **MapFieldTo:** `PaymSched`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymSpec`
        - **MapFieldTo:** `PaymSpec`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymTermId`
        - **MapFieldTo:** `PaymTermId`
      - **AxTableMappingConnection:**
        - **MapField:** `Rfc_MX`
        - **MapFieldTo:** `Rfc_MX`
      - **AxTableMappingConnection:**
        - **MapField:** `TaxGroup`
        - **MapFieldTo:** `TaxGroup`
      - **AxTableMappingConnection:**
        - **MapField:** `TaxPeriodPaymentCode_PL`
        - **MapFieldTo:** `TaxPeriodPaymentCode_PL`
      - **AxTableMappingConnection:**
        - **MapField:** `TaxWithholdCalculate`
        - **MapFieldTo:** `TaxWithholdCalculate_TH`
      - **AxTableMappingConnection:**
        - **MapField:** `TaxWithholdGroup`
        - **MapFieldTo:** `TaxWithholdGroup_TH`
      - **AxTableMappingConnection:**
        - **MapField:** `VATNum`
        - **MapFieldTo:** `VATNum`
  - **AxTableMapping:**
    - **MappingTable:** `DimensionDefaultMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `DefaultDimension`
        - **MapFieldTo:** `DefaultDimension`
  - **AxTableMapping:**
    - **MappingTable:** `DirPartyMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `Account`
        - **MapFieldTo:** `AccountNum`
      - **AxTableMappingConnection:**
        - **MapField:** `Party`
        - **MapFieldTo:** `Party`
  - **AxTableMapping:**
    - **MappingTable:** `InventStorageDimMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `InventLocationId`
        - **MapFieldTo:** `InventLocation`
      - **AxTableMappingConnection:**
        - **MapField:** `InventSiteId`
        - **MapFieldTo:** `InventSiteId`
  - **AxTableMapping:**
    - **MappingTable:** `PaymModeMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `PaymMode`
        - **MapFieldTo:** `PaymMode`
      - **AxTableMappingConnection:**
        - **MapField:** `PaymSpec`
        - **MapFieldTo:** `PaymSpec`
  - **AxTableMapping:**
    - **MappingTable:** `SysExtensionSerializerMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `PackedExtensions`
        - **MapFieldTo:** `PackedExtensions`
      - **AxTableMappingConnection:**
        - **MapField:** `PackedPrioritizedIdList`
  - **AxTableMapping:**
    - **MappingTable:** `CustVendFiscalEstablishmentMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `Account`
        - **MapFieldTo:** `AccountNum`
      - **AxTableMappingConnection:**
        - **MapField:** `CNPJCPFNum`
        - **MapFieldTo:** `CNPJCPFNum_BR`
      - **AxTableMappingConnection:**
        - **MapField:** `IENum`
        - **MapFieldTo:** `IENum_BR`
  - **AxTableMapping:**
    - **MappingTable:** `TaxExemptVATNumMap`
    - **Connections:**
      - **AxTableMappingConnection:**
        - **MapField:** `VATNum`
        - **MapFieldTo:** `VATNum`
      - **AxTableMappingConnection:**
        - **MapField:** `VATNumRecId`
        - **MapFieldTo:** `VATNumRecId`
      - **AxTableMappingConnection:**
        - **MapField:** `VATNumTableType`
        - **MapFieldTo:** `VATNumTableType`
- **Relations:**
  - **AxTableRelation:**
    - **Name:** `BankAccounts`
    - **Cardinality:** `ZeroMore`
    - **EntityRelationshipRole:** `@SYS123560`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `CustBankAccount`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `BankAccounts`
    - **RelationshipType:** `Aggregation`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `AccountNum`
        - **Field:** `AccountNum`
        - **RelatedField:** `CustAccount`
      - **AxTableRelationConstraint:**
        - **Name:** `BankAccount`
        - **Field:** `BankAccount`
        - **RelatedField:** `AccountID`
  - **AxTableRelation:**
    - **Name:** `BankAccounts_LV`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `CustBankAccount`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Aggregation`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `AccountNum`
        - **Field:** `AccountNum`
        - **RelatedField:** `CustAccount`
      - **AxTableRelationConstraint:**
        - **Name:** `IntBank_LV`
        - **Field:** `IntBank_LV`
        - **RelatedField:** `AccountID`
  - **AxTableRelation:**
    - **Name:** `BankCentralBankPurpose`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `BankCentralBankPurpose`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `BankCentralBankPurposeCode`
        - **SourceEDT:** `BankCentralBankPurposeCode`
        - **Field:** `BankCentralBankPurposeCode`
        - **RelatedField:** `Code`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `BankCustPaymIdTable`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `BankCustPaymIdTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `BankCustPaymIdTable`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `BankCustPaymIdTable`
        - **Field:** `BankCustPaymIdTable`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `BirthCounty`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `LogisticsAddressCounty`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `LogisticsAddressCounty`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `BirthCountyCode_IT`
        - **SourceEDT:** `AddressCountyCode_IT`
        - **Field:** `BirthCountyCode_IT`
        - **RelatedField:** `CountyCode_IT`
  - **AxTableRelation:**
    - **Name:** `CashDisc`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CashDisc`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CashDisc`
        - **SourceEDT:** `CustCashDiscCode`
        - **Field:** `CashDisc`
        - **RelatedField:** `CashDiscCode`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `CommissionCustomerGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CommissionCustomerGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CommissionGroup`
        - **SourceEDT:** `CommissCustomerGroup`
        - **Field:** `CommissionGroup`
        - **RelatedField:** `GroupId`
    - **Index:** `GroupIdx`
  - **AxTableRelation:**
    - **Name:** `CommissionSalesGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CommissionSalesGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SalesGroup`
        - **SourceEDT:** `CommissSalesGroup`
        - **Field:** `SalesGroup`
        - **RelatedField:** `GroupId`
    - **Index:** `GroupIdx`
  - **AxTableRelation:**
    - **Name:** `CompanyNAFCode`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `CompanyNAFCode`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `CompanyNAFCode`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CompanyNAFCode`
        - **Field:** `CompanyNAFCode`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `ContactPerson`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `ContactPerson`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `ContactPerson`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `ContactPersonId`
        - **SourceEDT:** `ContactPersonId`
        - **Field:** `ContactPersonId`
        - **RelatedField:** `ContactPersonId`
    - **Index:** `ContactPersonId`
  - **AxTableRelation:**
    - **Name:** `Currency`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `Currency`
    - **RelatedTableCardinality:** `ExactlyOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Currency`
        - **SourceEDT:** `CustCurrencyCode`
        - **Field:** `Currency`
        - **RelatedField:** `CurrencyCode`
    - **Index:** `CurrencyCodeIdx`
  - **AxTableRelation:**
    - **Name:** `CustClassificationGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CustClassificationGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `CustClassificationGroup`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CustClassificationId`
        - **SourceEDT:** `CustClassificationId`
        - **Field:** `CustClassificationId`
        - **RelatedField:** `Code`
  - **AxTableRelation:**
    - **Name:** `CustDirectDebitMandate`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `CustDirectDebitMandate`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DefaultDirectDebitMandate`
        - **Field:** `DefaultDirectDebitMandate`
        - **RelatedField:** `RecId`
  - **AxTableRelation:**
    - **Name:** `CustFineSetup_BR`
    - **Cardinality:** `ZeroMore`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `CustFineSetup_BR`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `FineCode_BR`
        - **Field:** `FineCode_BR`
        - **RelatedField:** `FineCode`
    - **Index:** `FineCode`
  - **AxTableRelation:**
    - **Name:** `CustGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `CustGroup`
    - **RelatedTableCardinality:** `ExactlyOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CustGroup`
        - **SourceEDT:** `CustGroupId`
        - **Field:** `CustGroup`
        - **RelatedField:** `CustGroup`
    - **Index:** `GroupIdx`
  - **AxTableRelation:**
    - **Name:** `CustInterestSetup_BR`
    - **Cardinality:** `ZeroMore`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `CustInterestSetup_BR`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `InterestCode_BR`
        - **Field:** `InterestCode_BR`
        - **RelatedField:** `InterestCode`
    - **Index:** `InterestCode`
  - **AxTableRelation:**
    - **Name:** `CustPaymentModeSpec`
    - **Cardinality:** `ZeroMore`
    - **EntityRelationshipRole:** `@SYS125116`
    - **RelatedTable:** `CustPaymModeSpec`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `CustPaymentModeSpec`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PaymMode`
        - **Field:** `PaymMode`
        - **RelatedField:** `PaymMode`
      - **AxTableRelationConstraint:**
        - **Name:** `PaymSpec`
        - **Field:** `PaymSpec`
        - **RelatedField:** `Specification`
  - **AxTableRelation:**
    - **Name:** `CustPaymModeTable`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CustPaymModeTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PaymMode`
        - **SourceEDT:** `CustPaymMode`
        - **Field:** `PaymMode`
        - **RelatedField:** `PaymMode`
    - **Index:** `PaymModeIdx`
  - **AxTableRelation:**
    - **Name:** `CustStatisticsGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `CustStatisticsGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `StatisticsGroup`
        - **SourceEDT:** `CustStatGroupId`
        - **Field:** `StatisticsGroup`
        - **RelatedField:** `CustStatisticsGroup`
    - **Index:** `GroupIdx`
  - **AxTableRelation:**
    - **Name:** `CustTable_FactoringAccount`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CustTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `CustTable_FactoringAccount`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `FactoringAccount`
        - **SourceEDT:** `BankCustFactoringAccount`
        - **Field:** `FactoringAccount`
        - **RelatedField:** `AccountNum`
    - **Index:** `AccountIdx`
  - **AxTableRelation:**
    - **Name:** `CustTable_InvoiceAccount`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CustTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `CustTable_InvoiceAccount`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `InvoiceAccount`
        - **SourceEDT:** `CustInvoiceAccount`
        - **Field:** `InvoiceAccount`
        - **RelatedField:** `AccountNum`
    - **Index:** `AccountIdx`
  - **AxTableRelation:**
    - **Name:** `CustTradingPartnerCode`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `CustTradingPartnerCode`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `CustTradingPartnerCode`
    - **RelationshipType:** `Association`
    - **Role:** `CustTradingPartnerCode_CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CustTradingPartnerCode`
        - **SourceEDT:** `TradingPartnerCodeRecId`
        - **Field:** `CustTradingPartnerCode`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `CustVendItemGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `CustVendItemGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CustItemGroupId`
        - **SourceEDT:** `CustItemGroupId`
        - **Field:** `CustItemGroupId`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustItemGroupId`
        - **RelatedField:** `Module`
        - **ValueStr:** `ModuleInventCustVend::Cust`
  - **AxTableRelation:**
    - **Name:** `DefaultDimension`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `DimensionAttributeValueSet`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `DefaultDimension`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DefaultDimension`
        - **Field:** `DefaultDimension`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `DestinationCode`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `DestinationCode`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DestinationCodeId`
        - **SourceEDT:** `CustDestinationCodeId`
        - **Field:** `DestinationCodeId`
        - **RelatedField:** `DestinationCodeId`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `DirAddressBookParty`
    - **RelatedTable:** `DirAddressBookPartyAllView`
    - **RelatedTableRole:** `DirAddressBookPartyAllView`
    - **RelationshipType:** `Link`
    - **Role:** `CustTable`
    - **Validate:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Party`
        - **Field:** `Party`
        - **RelatedField:** `Party`
  - **AxTableRelation:**
    - **Name:** `DirPartyTable_FK`
    - **Cardinality:** `ZeroOne`
    - **CreateNavigationPropertyMethods:** `Yes`
    - **RelatedTable:** `DirPartyTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `DirPartyTable_FK`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Party`
        - **Field:** `Party`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `DirPartyView`
    - **EntityRelationshipRole:** `@SYS124637`
    - **RelatedTable:** `DirPartyView`
    - **RelatedTableRole:** `DirPartyView`
    - **RelationshipType:** `Link`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Validate:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Party`
        - **Field:** `Party`
        - **RelatedField:** `Party`
  - **AxTableRelation:**
    - **Name:** `DlvMode`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `DlvMode`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DlvMode`
        - **SourceEDT:** `CustDlvModeId`
        - **Field:** `DlvMode`
        - **RelatedField:** `Code`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `DlvReason`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `DlvReason`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DlvReason`
        - **SourceEDT:** `DlvReasonId`
        - **Field:** `DlvReason`
        - **RelatedField:** `Code`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `DlvTerm`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `DlvTerm`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DlvTerm`
        - **SourceEDT:** `CustDlvTermId`
        - **Field:** `DlvTerm`
        - **RelatedField:** `Code`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `InventLocation`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `InventLocation`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `InventLocation`
        - **SourceEDT:** `InventLocationId`
        - **Field:** `InventLocation`
        - **RelatedField:** `InventLocationId`
    - **Index:** `InventLocationIdx`
  - **AxTableRelation:**
    - **Name:** `InventOrderEntryDeadlineGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `InventOrderEntryDeadlineGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `OrderEntryDeadlineGroupId`
        - **SourceEDT:** `InventOrderEntryDeadlineGroupId`
        - **Field:** `OrderEntryDeadlineGroupId`
        - **RelatedField:** `DeadlineGroupId`
    - **Index:** `DeadlineGroupIdx`
  - **AxTableRelation:**
    - **Name:** `InventProfile_RU`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `InventProfile_RU`
    - **RelatedTableCardinality:** `ExactlyOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `InventProfileId_RU`
        - **SourceEDT:** `InventProfileId_RU`
        - **Field:** `InventProfileId_RU`
        - **RelatedField:** `InventProfileId`
    - **Index:** `InventProfileIdx`
  - **AxTableRelation:**
    - **Name:** `InventSite`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `InventSite`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `InventSiteId`
        - **SourceEDT:** `InventSiteId`
        - **Field:** `InventSiteId`
        - **RelatedField:** `SiteId`
    - **Index:** `SiteIdx`
  - **AxTableRelation:**
    - **Name:** `LineOfBusiness`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `LineOfBusiness`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `LineOfBusinessId`
        - **SourceEDT:** `CustLineOfBusinessId`
        - **Field:** `LineOfBusinessId`
        - **RelatedField:** `LineOfBusinessId`
    - **Index:** `CodeIdx`
  - **AxTableRelation:**
    - **Name:** `LogisticsAddressCountryRegion`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `LogisticsAddressCountryRegion`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PartyCountry`
        - **SourceEDT:** `AddressCountryRegionId`
        - **Field:** `PartyCountry`
        - **RelatedField:** `CountryRegionId`
    - **Index:** `CountryRegionIdx`
  - **AxTableRelation:**
    - **Name:** `LogisticsAddressCountryRegion_RFIT`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `LogisticsAddressCountryRegion`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `LogisticsAddressCountryRegion_RFIT`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable_RFIT`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `ResidenceForeignCountryRegionId_IT`
        - **SourceEDT:** `LogisticsAddressCountryRegionId`
        - **Field:** `ResidenceForeignCountryRegionId_IT`
        - **RelatedField:** `CountryRegionId`
    - **Index:** `CountryRegionIdx`
  - **AxTableRelation:**
    - **Name:** `LvPaymTransCodes`
    - **Cardinality:** `ZeroOne`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `LvPaymTransCodes`
    - **RelatedTableCardinality:** `ExactlyOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `LvPaymTransCodes`
        - **Field:** `LvPaymTransCodes`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `MainContactWorker`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `HcmWorker`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `HcmWorker`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `MainContactWorker`
        - **Field:** `MainContactWorker`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `MarkupGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `MarkupGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `MarkupGroup`
        - **SourceEDT:** `CustMarkupGroupId`
        - **Field:** `MarkupGroup`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustMarkupGroupId`
        - **RelatedField:** `Module`
        - **ValueStr:** `MarkupModuleType::Cust`
  - **AxTableRelation:**
    - **Name:** `NumberSequenceGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `NumberSequenceGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `numberSequenceGroup`
        - **SourceEDT:** `NumberSequenceGroupId`
        - **Field:** `numberSequenceGroup`
        - **RelatedField:** `numberSequenceGroupId`
    - **Index:** `groupId`
  - **AxTableRelation:**
    - **Name:** `PartyState`
    - **Cardinality:** `ZeroMore`
    - **EntityRelationshipRole:** `@SYS125117`
    - **RelatedTable:** `LogisticsAddressState`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PartyState`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PartyCountry`
        - **Field:** `PartyCountry`
        - **RelatedField:** `CountryRegionId`
      - **AxTableRelationConstraint:**
        - **Name:** `PartyState`
        - **Field:** `PartyState`
        - **RelatedField:** `StateId`
  - **AxTableRelation:**
    - **Name:** `PassportIssuerLogisticsAddressCountry_HU`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `LogisticsAddressCountryRegion`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PassportIssuerCountryRegion`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `IssuerCountry_HU`
        - **SourceEDT:** `AddressCountryRegionId`
        - **Field:** `IssuerCountry_HU`
        - **RelatedField:** `CountryRegionId`
    - **Index:** `CountryRegionIdx`
  - **AxTableRelation:**
    - **Name:** `PaymDay`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PaymDay`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PaymDayId`
        - **SourceEDT:** `CustPaymDayId`
        - **Field:** `PaymDayId`
        - **RelatedField:** `PaymDayId`
    - **Index:** `PaymDayIdx`
  - **AxTableRelation:**
    - **Name:** `PaymSched`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `PaymSched`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PaymSched`
        - **SourceEDT:** `PaymSchedId`
        - **Field:** `PaymSched`
        - **RelatedField:** `Name`
    - **Index:** `NameIdx`
  - **AxTableRelation:**
    - **Name:** `PaymTerm_ClearingPeriod`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PaymTerm`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PaymTerm_ClearingPeriod`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `ClearingPeriod`
        - **SourceEDT:** `CustClearingPeriod`
        - **Field:** `ClearingPeriod`
        - **RelatedField:** `PaymTermId`
    - **Index:** `TermIdx`
  - **AxTableRelation:**
    - **Name:** `PaymTerm_PaymTermId`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PaymTerm`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PaymTerm_PaymTermId`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PaymTermId`
        - **SourceEDT:** `CustPaymTermId`
        - **Field:** `PaymTermId`
        - **RelatedField:** `PaymTermId`
    - **Index:** `TermIdx`
  - **AxTableRelation:**
    - **Name:** `PdsCustRebateGroup`
    - **Cardinality:** `ZeroMore`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `PdsCustRebateGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PdsCustRebateGroupId`
        - **Field:** `PdsCustRebateGroupId`
        - **RelatedField:** `PdsCustRebateGroupId`
  - **AxTableRelation:**
    - **Name:** `PdsRebateProgramTMATable`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `PdsRebateProgramTMATable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PdsRebateTMAGroup`
        - **Field:** `PdsRebateTMAGroup`
        - **RelatedField:** `PdsRebateProgramTMAGroup`
  - **AxTableRelation:**
    - **Name:** `PlTaxDueTable`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `PlTaxDueTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PlTaxDueTable`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `TaxPeriodPaymentCode_PL`
        - **Field:** `TaxPeriodPaymentCode_PL`
        - **RelatedField:** `TaxPeriodPaymentCode`
    - **Index:** `TaxPeriodPaymentCodeIdx`
  - **AxTableRelation:**
    - **Name:** `PriceDiscGroup_EndDisc`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PriceDiscGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PriceDiscGroup_EndDisc`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `EndDisc`
        - **SourceEDT:** `CustEndDiscCode`
        - **Field:** `EndDisc`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Type_Extern`
        - **SourceEDT:** `CustEndDiscCode`
        - **RelatedField:** `Type`
        - **ValueStr:** `PriceGroupType::EndDiscGroup`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustEndDiscCode`
        - **RelatedField:** `Module`
        - **ValueStr:** `ModuleInventCustVend::Cust`
  - **AxTableRelation:**
    - **Name:** `PriceDiscGroup_LineDisc`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PriceDiscGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PriceDiscGroup_LineDisc`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `LineDisc`
        - **SourceEDT:** `CustLineDiscCode`
        - **Field:** `LineDisc`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Type_Extern`
        - **SourceEDT:** `CustLineDiscCode`
        - **RelatedField:** `Type`
        - **ValueStr:** `PriceGroupType::LineDiscGroup`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustLineDiscCode`
        - **RelatedField:** `Module`
        - **ValueStr:** `ModuleInventCustVend::Cust`
  - **AxTableRelation:**
    - **Name:** `PriceDiscGroup_MultiLineDisc`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PriceDiscGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PriceDiscGroup_MultiLineDisc`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `MultiLineDisc`
        - **SourceEDT:** `CustMultiLineDiscCode`
        - **Field:** `MultiLineDisc`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Type_Extern`
        - **SourceEDT:** `CustMultiLineDiscCode`
        - **RelatedField:** `Type`
        - **ValueStr:** `PriceGroupType::MultiLineDiscGroup`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustMultiLineDiscCode`
        - **RelatedField:** `Module`
        - **ValueStr:** `ModuleInventCustVend::Cust`
  - **AxTableRelation:**
    - **Name:** `PriceDiscGroup_PriceGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `PriceDiscGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `PriceDiscGroup_PriceGroup`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `PriceGroup`
        - **SourceEDT:** `CustPriceGroup`
        - **Field:** `PriceGroup`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Type_Extern`
        - **SourceEDT:** `CustPriceGroup`
        - **RelatedField:** `Type`
        - **ValueStr:** `PriceGroupType::PriceGroup`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustPriceGroup`
        - **RelatedField:** `Module`
        - **ValueStr:** `ModuleInventCustVend::Cust`
  - **AxTableRelation:**
    - **Name:** `SalesPool`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `SalesPool`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SalesPoolId`
        - **SourceEDT:** `CustSalesPoolId`
        - **Field:** `SalesPoolId`
        - **RelatedField:** `SalesPoolId`
    - **Index:** `SalesPoolIdx`
  - **AxTableRelation:**
    - **Name:** `ShipCarrierTable`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `ShipCarrierTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `ShipCarrierId`
        - **SourceEDT:** `ShipCarrierId`
        - **Field:** `ShipCarrierId`
        - **RelatedField:** `CarrierId`
    - **Index:** `IdIdx`
  - **AxTableRelation:**
    - **Name:** `smmBusRelChainGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `smmBusRelChainGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CompanyChainId`
        - **SourceEDT:** `smmChainId`
        - **Field:** `CompanyChainId`
        - **RelatedField:** `ChainId`
    - **Index:** `ChainIdx`
  - **AxTableRelation:**
    - **Name:** `smmBusRelSalesDistrictGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `smmBusRelSalesDistrictGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SalesDistrictId`
        - **SourceEDT:** `smmSalesDistrictId`
        - **Field:** `SalesDistrictId`
        - **RelatedField:** `SalesDistrictId`
    - **Index:** `SalesDistrictIdx`
  - **AxTableRelation:**
    - **Name:** `smmBusRelSegmentGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `smmBusRelSegmentGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SegmentId`
        - **SourceEDT:** `smmSegmentId`
        - **Field:** `SegmentId`
        - **RelatedField:** `SegmentId`
    - **Index:** `SegmentIdx`
  - **AxTableRelation:**
    - **Name:** `smmBusRelSubSegmentGroup`
    - **Cardinality:** `ZeroMore`
    - **EntityRelationshipRole:** `@SYS124723`
    - **RelatedTable:** `smmBusRelSubSegmentGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `smmBusRelSubSegmentGroup`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SegmentId`
        - **Field:** `SegmentId`
        - **RelatedField:** `SegmentId`
      - **AxTableRelationConstraint:**
        - **Name:** `SubsegmentId`
        - **Field:** `SubsegmentId`
        - **RelatedField:** `SubsegmentId`
  - **AxTableRelation:**
    - **Name:** `SuppItemGroup`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `SuppItemGroup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SuppItemGroupId`
        - **SourceEDT:** `CustSuppItemGroupId`
        - **Field:** `SuppItemGroupId`
        - **RelatedField:** `GroupId`
      - **AxTableRelationConstraint:**
        - **Name:** `Module_Extern`
        - **SourceEDT:** `CustSuppItemGroupId`
        - **RelatedField:** `Module`
        - **ValueStr:** `ModuleInventCustVend::Cust`
  - **AxTableRelation:**
    - **Name:** `TaxGroupHeading`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `TaxGroupHeading`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `TaxGroup`
        - **SourceEDT:** `TaxGroup`
        - **Field:** `TaxGroup`
        - **RelatedField:** `TaxGroup`
    - **Index:** `TaxGroupIdx`
  - **AxTableRelation:**
    - **Name:** `TaxWithholdGroupHeading`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `TaxWithholdGroupHeading`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `TaxWithholdGroup_TH`
        - **SourceEDT:** `TaxWithholdGroup`
        - **Field:** `TaxWithholdGroup_TH`
        - **RelatedField:** `TaxWithholdGroup`
    - **Index:** `TaxWithholdGroupIdx`
  - **AxTableRelation:**
    - **Name:** `VatNum`
    - **Cardinality:** `ZeroMore`
    - **EntityRelationshipRole:** `@SYS125042`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `TaxVATNumTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `VatNum`
    - **RelationshipType:** `Association`
    - **Role:** `CustTable`
    - **UseDefaultRoleNames:** `No`
    - **Validate:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `VATNum`
        - **Field:** `VATNum`
        - **RelatedField:** `VATNum`
  - **AxTableRelation:**
    - **Name:** `VendTable`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `VendTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `VendAccount`
        - **SourceEDT:** `VendAccount`
        - **Field:** `VendAccount`
        - **RelatedField:** `AccountNum`
    - **Index:** `AccountIdx`
  - **AxTableRelation:**
    - **Name:** `WHSInventStatus`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `WHSInventStatus`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `DefaultInventStatusId`
        - **SourceEDT:** `WHSDefaultStatusId`
        - **Field:** `DefaultInventStatusId`
        - **RelatedField:** `InventStatusId`
  - **AxTableRelation:**
    - **Name:** `WorkCalendarTable`
    - **Cardinality:** `ZeroMore`
    - **EDTRelation:** `Yes`
    - **RelatedTable:** `WorkCalendarTable`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SalesCalendarId`
        - **SourceEDT:** `SalesCalendarId`
        - **Field:** `SalesCalendarId`
        - **RelatedField:** `CalendarId`
    - **Index:** `CalendarIdx`
  - **AxTableRelation:**
    - **Name:** `DocuRef`
    - **Cardinality:** `ZeroOne`
    - **EntityRelationshipRole:** `@SYS123524`
    - **RelatedTable:** `DocuRef`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelatedTableRole:** `IsDocumentOf`
    - **RelationshipType:** `Association`
    - **Role:** `IsCustFor`
    - **UseDefaultRoleNames:** `No`
    - **Validate:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Party`
        - **Field:** `Party`
        - **RelatedField:** `Party`
  - **AxTableRelation:**
    - **Name:** `TaxGSTReliefGroupHeading_MY`
    - **Cardinality:** `ZeroMore`
    - **RelatedTable:** `TaxGSTReliefGroupHeading_MY`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `TaxGSTReliefGroupHeading_MY`
        - **Field:** `TaxGSTReliefGroupHeading_MY`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `CustCollectionsCaseDetail`
    - **RelatedTable:** `CustCollectionsCaseDetail`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Party`
        - **Field:** `Party`
        - **RelatedField:** `Party`
  - **AxTableRelation:**
    - **Name:** `CustWriteoffAccountRelation`
    - **Cardinality:** `ZeroOne`
    - **OnDelete:** `Restricted`
    - **RelatedTable:** `CustWriteOffFinancialReasonsSetup`
    - **RelatedTableCardinality:** `ZeroOne`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `CustWriteOffRefRecId`
        - **Field:** `CustWriteOffRefRecId`
        - **RelatedField:** `RecId`
    - **Index:** `RecId`
  - **AxTableRelation:**
    - **Name:** `ContactPersons`
    - **RelatedTable:** `ContactPerson`
    - **RelatedTableRole:** `ContactPersons`
    - **RelationshipType:** `Link`
    - **Role:** `Customer`
    - **UseDefaultRoleNames:** `No`
    - **Validate:** `No`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `Party`
        - **Field:** `Party`
        - **RelatedField:** `ContactForParty`
  - **AxTableRelation:**
    - **Name:** `SATPaymMethod_MX`
    - **Cardinality:** `ExactlyOne`
    - **RelatedTable:** `EInvoiceExtCodeTable_MX`
    - **RelatedTableCardinality:** `ExactlyOne`
    - **RelatedTableRole:** `ExtCodeTablePaymMethod`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SATPaymMethod_MX`
        - **Field:** `SATPaymMethod_MX`
        - **RelatedField:** `CodeId`
      - **AxTableRelationConstraint:**
        - **Name:** `CodeType`
        - **RelatedField:** `CodeType`
        - **Value:** `4`
  - **AxTableRelation:**
    - **Name:** `SATPurpose_MX`
    - **Cardinality:** `ExactlyOne`
    - **RelatedTable:** `EInvoiceExtCodeTable_MX`
    - **RelatedTableCardinality:** `ExactlyOne`
    - **RelatedTableRole:** `ExtCodeTablePurpose`
    - **RelationshipType:** `Association`
    - **Constraints:**
      - **AxTableRelationConstraint:**
        - **Name:** `SATPurpose_MX`
        - **Field:** `SATPurpose_MX`
        - **RelatedField:** `CodeId`
      - **AxTableRelationConstraint:**
        - **Name:** `CodeType`
        - **RelatedField:** `CodeType`
        - **Value:** `3`
- **StateMachines:**
