#
# This is a Shiny web application. You can run the application by clicking
# the 'Run App' button above.
#
# Find out more about building applications with Shiny here:
#
#    http://shiny.rstudio.com/
#

library(shiny)
library(devtools)

source('shiny_function.R')
#source_url('bit.ly/ceushiny')


library("jsonlite")
library("data.table")
library("httr")
library("rtsdata")
library("DT")
library("TTR")
library("plotly")
library("shinyjs")
library("ggplot2")

library(shinydashboard)

ui <- dashboardPage(
    #tags$head(includeScript("google-analytics.js")),
    dashboardHeader(title = 'Stocks'),
    dashboardSidebar(
        sidebarMenu(
            menuItem('Control', tabName = 'control', icon = icon('database'),
                     uiOutput('ui_tickers'),
                     dateRangeInput(inputId = 'date', label = 'Select Date', start = Sys.Date() - 365, end = Sys.Date())
            ), # press f1 to check library
            menuItem('Plot', tabName = 'plot')
        )
        
        
    ),
    dashboardBody(
        tabItems(
            tabItem(tabName = 'control',
                    h1('select date and ticker')
            ),
            tabItem(tabName = 'plot',
                    DT::dataTableOutput('ticker_data'),
                    plotlyOutput('data_plotly', width = '60%', height = '800px')),
            infoBoxOutput('performance_infobox')
            
        )
        
    )
)


server <- function(input, output) {
    df <- get_sp500()
    
    output$ui_tickers <- renderUI({
        selectInput(inputId = 'ticker', label = 'Tickers', choices = setNames(df$name, df$description)
                    , multiple = FALSE)
    })
    
    output$selected_ticker_text <- renderText(input$ticker)
    
    
    reactive_df <- reactive({
        t <- get_data_by_ticker_and_date(ticker = input$ticker, start_date = input$date[1], end_date = input$date[2])
        #t$date <- as.characters(t$date)
        return(t)
    })
    
    output$ticker_data <- DT::renderDataTable({
        my_render_df(reactive_df())
    })
    
    output$data_plotly <- plotly::renderPlotly(
        {
            get_plot_of_data(reactive_df())
        }
    )
    
    output$data_ggplot <- renderPlot({
        ggplot(reactive_df(), aes(date, close)) + geom_line() + theme_bw()
    })
    # these are the two functions that we need
    #plotly::renderPlotly()
    #plotly::plotlyOutput()
    output$performance_infobox <- renderInfoBox({
        infoBox('Positive performance', paste0(round((sum(df$change>0) / nrow(df) * 100), 0), '%'))
    })
    
}


#renderDataTable()
#dataTableOutput()

# Run the application 
shinyApp(ui = ui, server = server)

#library(rsconnect)
