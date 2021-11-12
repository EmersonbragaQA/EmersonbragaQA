*Variables
${WEBSITE}                            http://demoqa.com
${WEBPAGE}                            /login
${LOGIN_USER}                         css=#userName
${LOGIN_PASS}                         css=#password
${USERNAME}                           emersonbraga
${PASSWORD}                           Paidoben123#
${LOGIN_BUTTON}                       css=#login
${STORE}                              css=#gotoStore
${BOOK}                               xpath=//*[@id="app"]/div/div/div[2]/div[2]/div[1]/div[2]/div[1]/div[2]/div[1]/div/div[2]
${PROFILE}                            css=#item-3
${BOOK_NAME}                          css=#userName-value
${ADD_COLLECTION}                     css=#app > div > div > div.row > div.col-12.mt-4.col-md-6 > div.books-wrapper > div.profile-wrapper > div.mt-2.fullButtonWrap.row > div.text-right.fullButton
${DELETE}                             css=.di > #submit
${DELETE_PRE_VALIDATION}              css=body > div.fade.modal.show > div > div > div.modal-body
${DELETE_VALIDATION}                  css=#closeSmallModal-ok

*Keywords
Log into bookstore's application with user ${USER} and pass ${PASS}
  Open Browser  ${WEBSITE}${WEBPAGE}      Chrome
  Maximize Browser Window
  Wait before input text                  ${LOGIN_USER}   ${USERNAME}
  Wait before input text                  ${LOGIN_PASS}   ${PASSWORD}
  Wait before clicking                    ${LOGIN_BUTTON}

At the book store page, add ${BOOK_TITLE} to collection
  Sleep  3
  Wait and scroll before clicking      ${STORE}
  Go To                                ${WEBSITE}/books?book=9781449325862
  Wait and scroll before clicking      ${ADD_COLLECTION}
  Handle Alert

Validate that ${BOOK_TITLE} is being displayed on collection
  Go To                                 ${WEBSITE}/profile
  Wait Until Element is Visible         ${BOOK}
  Element Should Contain                ${BOOK}               Git Pocket Guide

Clean user's collection list
  Wait and scroll before clicking       ${DELETE}
  Wait before clicking                  ${DELETE_VALIDATION}
  Handle alert

#In order for this API test to run, robotframework-requests library should be properly installed
#http://marketsquare.github.io/robotframework-requests
User authentication request is Ok (200)
  create session  AddData  ${WEBSITE}
  &{BODY} =       Create Dictionary   username=emersonbraga            password=Paidoben123#
  ${RESPONSE} =   POST On Session     AddData  /Account/v1/Authorized  data=${BODY}  expected_status=200

Bookstore request is Ok (200)
  ${RESPONSE} =  GET  ${WEBSITE}/BookStore/v1/Books  expected_status=200
  
Wait before clicking
  [Arguments]                       ${CLICKELEMENT}
  Wait Until Element is Visible     ${CLICKELEMENT}
  Wait Until Element is Enabled     ${CLICKELEMENT}
  Click Element                     ${CLICKELEMENT}

#There are some third-party ads being displayed on the website and we need to bypass them using the command window.ScrollTo
Wait and scroll before clicking 
  [Arguments]                       ${SCROLL}
  Execute Javascript                window.scrollTo(0, 575)
  Wait Until Element is Visible     ${SCROLL}
  Click Element                     ${SCROLL}

Wait before input text
  [Arguments]                       ${ELEMENT}  ${TEXT}
  Wait Until Element is Visible     ${ELEMENT}
  Input Text                        ${ELEMENT}  ${TEXT}
