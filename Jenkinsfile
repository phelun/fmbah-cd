#!/usr/bin/env groovy 

// Console Output Display Block 
def seperator60 = '\u2739' * 60
def seperator20 = '\u2739' * 20


node('misc') {
    echo "${seperator60}\n${seperator20} Clone repo to workspace \n${seperator60}"
    stage(deployer) {
        checkout scm 
    }
}


