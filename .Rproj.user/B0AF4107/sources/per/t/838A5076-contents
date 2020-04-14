library(jsonlite)
library(data.table)
library(httr)
library(shiny)
source('shiny_function.R')
#source_url('bit.ly/ceushiny')

#get_sp500()

ui <- fluidPage(
  uiOutput('ui_tickers'),
  textOutput('selected_ticker_text')
)
server <- function(input, output) {
  df <- get_sp500()
  output$ui_tickers <- renderUI({
    selectInput(inputId = 'ticker', label = 'Tickers', choices = setNames(df$name, df$description)
, multiple = FALSE)
  })
  
  output$selected_ticker_text <- renderText(input$ticker)

}

#get_data_by_ticker_and_date('TS')

renderDataTable()
dataTableOutput()

shinyApp(ui, server)